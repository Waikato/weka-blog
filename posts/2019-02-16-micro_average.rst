.. title: Micro averages in multi-class classification
.. slug: 2019-02-16-micro_average
.. date: 2019-02-16 13:48:00 UTC+12:00
.. tags: github
.. author: Eibe Frank
.. description: 
.. category: statistics

When evaluating multi-class classification models, Weka outputs a weighted average of the per-class precision, recall, and F-measure: it computes these statistics for each class individually, treating the corresponding class as the "positive" class and the union of the other classes as the negative class, and computes a weighted average of these per-class statistics, with a per-class weight that is equal to the proportion of data in that class. A recent question on the Weka mailing list was whether the software also outputs micro-averaged precision, recall, and F-measure for multi-class problems. It turns out that it does! To find out where these can be found, read on. 

.. TEASER_END

The answer is somewhat surprising. It turns out that for a multi-class classification problem (including two-class ones), micro-averaged precision, recall and F-measure are all the same and identical to classification accuracy as measured by the percentage of correctly classified instances. And the percentage of correctly classified instances is obviously something that is output by Weka!

Section 1: Micro-averaged precision
===================================

Let us consider precision first. For a single class, precision is the number of true positives TP for that class divided by the sum of the number of true positives TP and the number of false positives FP. The denominator, the sum of TP and FP, is equal to the number of predicted positives PP. Now, to compute the micro-averaged precision across a set of classes, we simply sum up the values of TP for the different classes to obtain the numerator for the calculation of (micro-averaged) precision and take the sum of the values of PP for the different classes as the denominator. For example, assuming two classes A and B, and denoting true positives, false positives, and predicted positives for A by TP(A), FN(A), and PP(A), and doing the same for B, we can write micro-averaged precision as

::

  (TP(A) + TP(B)) / (TP(A) + FP(A) + TP(B) + FP(B)) = (TP(A) + TP(B)) / (PP(A) + PP(B))

Looking at this more closely, we can easily see that this is actually the percentage of correctly classified instances, i.e., classification accuracy. The numerator is the sum of the diagonal elements of the corresponding confusion matrix, and the denominator is the sum of all the values in the confusion matrix (i.e., the total number of instances in the dataset). Hence, micro-averaged precision is exactly the same as the percentage of correct classifications for a two-class problem. Moreover, it is straightforward to generalise this to multi-class problems: we can simply extend the sums in the numerator and the denominator.

Section 2: Micro-averaged recall
================================

Let us consider recall next, which is defined as the number of true positives TP divided by the number of positives P, where P is equal to the number of true positives TP plus the number of false negatives FN. Similarly to micro-averaged precision, micro-averaged recall is calculated by summing the TP values across the different classes to obtain the numerator; the denominator is obtained by taking the sum of the number of positives P across all classes. Considering a two-class problem again, for simplicity, and denoting the number of false negatives for classes A and B by FN(A) and FN(B) respectively, and the number of positives by P(A) and P(B), we get the following for micro-averaged recall:

::

  (TP(A) + TP(B)) / (TP(A) + FN(A) + TP(B) + FN(B)) = (TP(A) + TP(B)) / (P(A) + P(B)) 

Now, as in the case of micro-averaged precision, the numerator is the number of correctly classified instances. The denominator is the sum of the class sizes. So, again, just like micro-averaged precision, micro-averaged recall is also equal to the percentage of correct classification. As above, by extending the sums in the numerator and the denominator when there are more than two classes, we can see that this equivalence holds in multi-class scenarios with more than two classes as well.

Section 3: Micro-averaged F-measure
===================================

What about micro-averaged F-measure? That's the harmonic mean of micro-averaged precision and recall. Both are equal to classification accuracy so the micro-averaged is also equal to classification accuracy.

Section 4: Micro-averaged false positive rate
=============================================

Another way to look at classification models is to consider their false positive rate and their true positive rate. What are the micro-averaged versions of those statistics in a multi-class problem? True positive rate is the same as recall so we already know that it is the same as classification accuracy. What about the micro-averaged false positive rate? In the two-class case, it is

::

  (FP(A) + FP(B)) / (FP(A) + TN(A) + FP(B) + TN(B))

and this is the same as error rate (the percentage of incorrectly classified instances). However, when there are more than two classes, this equivalence no longer holds.


