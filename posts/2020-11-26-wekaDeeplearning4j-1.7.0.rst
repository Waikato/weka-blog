.. title: New WekaDeeplearning4j Release - CNN explorer, saliency maps, progress manager, and model summaries 
.. slug: 2020-11-26-wekaDeeplearning4j-1.7.0
.. date: 2020-11-26 18:06:00 UTC+12:00
.. tags: github
.. author: Rhys Compton
.. description: 
.. category: packages

A new version of WekaDeeplearning4j, version 1.7.0, is available!

.. TEASER_END

.. figure:: ../../images/GUI.jpg
	     Dl4jCNNExplorer and saliency map generation.

*******
Dl4j Inference Panel & Dl4jCNNExplorer
*******

One major addition in **WekaDeeplearning4j** v1.7.0 is the new **Dl4jCNNExplorer** and the
associated GUI **Dl4j Inference Panel**. This brings real-time inference to the WEKA universe,
allowing you to quickly run an image classification CNN model on an image without having to
load an entire `.arff` file.

The **Dl4jCNNExplorer** supports both a custom-trained `Dl4jMlpClassifier` and a model from
the Model Zoo, so it can be used to verify your model's prediction capabilities
or simply play around with pretrained models and explore what state-of-the-art
architectures may work best for your domain.

Check out the `usage example <https://deeplearning.cms.waikato.ac.nz/examples/dl4j-inference/>`__
to see how easy it is to get started.

*******
Saliency Map Generation with ScoreCAM
*******

Another exciting new feature is the implementation of **ScoreCAM**, a saliency map generation technique.
This can be accessed through the `Dl4jCNNExplorer`, allowing you to not only perform prediction on an image,
but look at *what* in the image your model was using for prediction.

This can be invoked from the command-line, although the best user experience is to be had from the GUI using the
**Saliency Map Viewer**, which allows you to quickly customize the ScoreCAM target classes.

Check out the `usage example <https://deeplearning.cms.waikato.ac.nz/examples/dl4j-inference/#example-4-saliency-map-generation>`__
to see what new insights can be brought to your workflow.

*******
Progress Manager
*******

.. figure:: ../../images/ProgressManager.png
	    Progress manager.

We've created a simple---but effective---progress bar and added this to the long-running tasks
(model training, feature extraction, etc.). This provides a graphical indicator of progress and remaining
ETA for the current job so will make WEKA more usable for large jobs.

*******
Model Summaries
*******

We've also added `model summaries <https://deeplearning.cms.waikato.ac.nz/user-guide/model-zoo/#model-summaries>`__
to the documentation, which specify the different models and their layers. This can be useful for designing
your own architectures or with the `Dl4jMlpFilter`, when using intermediary layers for feature extraction.
