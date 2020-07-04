.. title: New WekaDeeplearning4j Release - Pretrained Models, Feature Extraction Update, and more 
.. slug: 2020-07-04-wekaDeeplearning4j-1.6.0
.. date: 2020-07-04 18:06:00 UTC+12:00
.. tags: github
.. author: Eibe Frank
.. description: 
.. category: packages
   
A new version of WekaDeeplearning4j, version 1.6.0, has just been released and brings with it a bunch of exciting new features.
       
.. TEASER_END

Pretrained Models
=================

The package previously already contained a model zoo--a set of model architectures designed by others (e.g., AlexNet, ResNet50)--but there was no easy way to use a pretrained checkpoint in Wdl4jMlpClassifier so training these models had to be done from scratch. The new release of WekaDeeplearning4j both expands the model zoo to contain over 30 models and provides an easy way to initialize them with pre-trained weights that have been made publicly available for these models. This makes it even easier to start playing with state-of-the-art neural networks (e.g., EfficientNet): not only do you not need programming experience--due to the WEKA GUI--but now you don't even need a beefy GPU to start getting useful results because these models can simply be used as feature extractors without any extra training, simply using the pre-trained weights.

Updated Dl4jMlpFilter
=====================

This filter allows you to use a model--which can now be a pretrained model from the Model Zoo--as a feature extractor, converting an image dataset into a numeric form that can be used with any off-the-shelf WEKA classifier. In the new version, the filter also supports multiple feature extraction layers: by default the last dense layer will be used, but you can alternatively choose any intermediary layer as well, concatenating the activations from the two layers. This opens up a huge new world of experimentation.

Image Dataset Conversion Script
===============================

Some image classification datasets come in a simple 'folder-organised' fashion, where collections of images are split into subfolders, with the name of each subfolder providing the class of all images within it. To make it easier to work with these types of image datasets, the package now includes the `ImageDirectoryLoader` which can load in datasets of this form.

Check out the `documentation <https://deeplearning.cms.waikato.ac.nz>`__ for more info on these new features!

