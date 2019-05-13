.. title: Oversampling and Undersampling
.. slug: 2019-01-30-sampling
.. date: 2019-01-30 11:10:00 UTC+12:00
.. tags: github
.. author: Eibe Frank
.. description: 
.. category: preprocessing

A frequent question of Weka users is how to implement oversampling or undersampling, which are two common strategies for dealing with imbalanced classes in classification problems. This post provides some explanation.

.. TEASER_END

When a binary classification problem has a lot less data in one class than in the other one, e.g., when learning from results of a medical test for which the vast majority of instances have a negative outcome and only a few return positive, some machine learning algorithms will simply learn to ignore the minority class and classify all cases into the majority class because this will trivially yield high classification accuracy. This kind of classification model is clearly not useful. Two common methods for combating this problem are undersampling of the majority class and oversampling of the minority class respectively.

Section 1: Undersampling the majority class
===========================================

There are two Weka filters that can be used to implement undersampling of the majority class: 

::

  weka.filters.supervised.instance.Resample

and

::

  weka.filters.supervised.instance.SpreadSubsample

The first one, ``weka.filters.supervised.instance.Resample``, uses the following expression to determine the number of instances to sample for a particular class ``i``:

.. code:: java

   int sampleSize = (int)((m_SampleSizePercent / 100.0)  * ((1 - m_BiasToUniformClass) * numInstancesPerClass[i] + m_BiasToUniformClass * data.numInstances() / numActualClasses));

where ``data.numInstances()`` gives the total number of instances in the dataset, ``numInstancesPerClass[i]`` holds the number of instances in class ``i`` and ``numActualClasses`` corresponds to the number of classes that actually occur in the dataset (some classes declared in an ARFF file may not have any instances in the data).

Based on this, to undersample the majority class so that both classes have the same number of instances, we can configure the filter to use  ``biasToUniformClass=1.0`` and ``sampleSizePercent=X``, where ``X/2`` is (approximately) the percentage of data that belongs to the minority class. We also need to configure the filter to perform sampling without replacement by applying ``noReplacement=true``.

For example, on the diabetes data that comes with the Weka distribution, we can apply the following command-line configuration of the filter:

::

   weka.filters.supervised.instance.Resample -B 1.0 -Z 69.8 -no-replacement

When you apply this approach in practice, you will probably need to fiddle with the ``-Z`` parameter (i.e., ``sampleSizePercent``) a bit to keep all the instances of the minority class. Watch out for something like ``*WARNING: Not enough instances of tested_positive for selected value of bias parameter in supervised Resample filter when sampling without replacement.*``. It means that the value specified by ``-Z`` is too large: there is an insufficient number of instances in the minority class in the data.

This seems unnecessary complicated, and it is. Instead of using ``weka.filters.supervised.instance.Resample``, a much easier way to achieve the same effect is to use ``weka.filters.supervised.SpreadSubsample`` instead, with ``distributionSpread=1.0``:

:: 

  weka.filters.supervised.instance.SpreadSubsample -M 1.0

Section 2: Oversampling the minority class
==========================================

Now, to achieve oversampling of the minority class, rather than undersampling of the majority class, so that both classes have the same number of instances, we need to return to ``weka.filters.supervsied.Resample`` and apply it with ``noReplacement=false``, ``biasToUniformClass=1.0``, and ``sampleSizePercent=Y``, where ``Y/2`` is (approximately) the percentage of data that belongs to the majority class. 

In the case of the diabetes data that comes with the Weka distribution, we would use

::

  weka.filters.supervised.instance.Resample -B 1.0 -Z 130.3

An important caveat is that this will apply sampling *with* replacement to the majority class as well, so it may not be ideal for your application! To get oversampling of the minority class and keep the majority class untouched, you may need to write your own program or use Weka's KnowledgeFlow GUI.

Section 3: Concluding remarks
=============================

So these are two basic tools for undersampling and oversampling in Weka. However, there is also ``weka.classifiers.meta.CostSensitiveClassifier``, which, when applied in default mode, will reweight the training instances to take a given misclassification cost matrix into account and then just use the classifier built from the reweighted data. The weights that are produced can be used directly by any classifier in Weka that implements the ``WeightedInstancesHandler`` interface (whether this is the case can be checked by inspecting the classifier's ``Capabilities``). If the base classifier that is specified inside ``CostSensitveClassifier`` does not implement this ``WeightedInstancesHandler`` interface, then the data will be resampled (with replacement) based on the weights by normalising the weights into a probability distribution that is used for sampling. Finally, there is ``weka.filters.supervised.instance.ClassBalancer``, a very simple filter that assigns instance weights so that each class of instances will have the same weight and the total sum of instance weights in the dataset remains unchanged. When ``weka.classifiers.meta.FilteredClassifier`` is applied in conjunction with this filter and the base classifier does not implement ``WeightedInstancesHandler`` then the weights will again be used to form a probability distribution for sampling with replacement. This will yield a training set where both classes are (approximately) balanced.

In addition to the above filters and classifiers, which sample instances or modify instance weights, it is also possible to apply a more sophisticated approach that generates new data in the minority class by interpolating between the training instances in that class. This is what the well-known SMOTE method does, and there is a package for Weka of that name that provides this functionality as a Weka filter.

A final (important) note: all supervised filters in Weka must be applied in conjunction with ``weka.classifiers.meta.FilteredClassifier`` so that the test data is processed without introducing bias into the performance estimates obtained by k-fold cross-validation or a percentage split evaluation! For example, in the case of the above filters, we should not modify the test data using the filters. Application of the ``FilteredClassifier`` will ensure that this does not happen.

