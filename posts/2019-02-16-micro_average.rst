.. title: Micro average
.. slug: 2019-02-16-micro_average
.. date: 2019-02-16 13:48:00 UTC+12:00
.. tags: draft
.. author: Eibe Frank
.. description: 
.. category: statistics

For a multi-class confusion matrix (including two-class situations like yours in this), micro-averaged precision, recall and F-measure are all the same and identical to classification accuracy as measured by the percentage of correctly classified instances (not shown in the output you attached, but obviously also output by WEKA).

That is particularly easy to see when you only have two classes as in your case. Micro-averaged precision for two classes A and B, denoting true positives, false negatives etc., for A by TP(A), FN(A), etc., and doing the same for B:

::

  (TP(A) + TP(B)) / (TP(A) + FP(A) + TP(B) + FP(B))

The numerator is the sum of the diagonal elements of the confusion matrix, and the denominator is the sum of all the values in the confusion matrix (i.e., the total number of instances in the dataset), so this is exactly the same as the percentage of correct classifications.

Micro-averaged recall is

::

  (TP(A) + TP(B)) / (TP(A) + FN(A) + TP(B) + FN(B))

and FN(A) = FP(B), FN(B) = FP(A), so micro-averaged recall is the same as micro-averaged precision. Hence, micro-averaged F-measure is also the same.

What about micro-averaged false positive rate (note that true positive rate is the same as recall, so we have already covered it)?

In the same two-class case, it is

::

  (FP(A) + FP(B)) / (FP(A) + TN(A) + FP(B) + TN(B))

and this is the same as error rate (the percentage of incorrectly classified instances).


