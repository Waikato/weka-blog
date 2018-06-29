.. title: Weka-Kt: Kotlin Extensions for Weka
.. slug: 2018-06-22-wekakotlin
.. date: 2018-06-22 08:00:00 UTC+12:00
.. tags: kotlin
.. author: Steven Lang 
.. description: 
.. category: Library

.. figure:: https://raw.githubusercontent.com/steven-lang/weka-kt/master/resources/Weka_3_kotlin_full.png
   :width: 300px
   :target: https://github.com/steven-lang/weka-kt
   :alt: Weka-Kt: Kotlin Extensions for Weka


##########################
Kotlin Extensions for Weka
##########################

Kotlin is a statically typed programming language for modern multiplatform applications running on the Java Virtual Machine. Its design philosophy is to be a concise and type-safe (e.g. support for non-nullable types) programming language. It supports both object-oriented and functional constructs. Other features include smart casting, operator overloading, higher-order functions, lambdas, and `extensions <https://kotlinlang.org/docs/reference/extensions.html>`_. The latter has led to the formation of a group of Kotlin extension libraries that primarily focuses on the syntactic improvement of other libraries' usages. A key example is `Android KTX <https://github.com/android/android-ktx>`_ library, developed by Google, which provides extensions for the Android framework. The purpose of this is to make Android development with Kotlin more concise, pleasant and idiomatic. 

*************
Code Examples
*************

Weka-Kt is inspired by the same ideas as Android KTX. Its goal is to make the use of Weka more convenient. Let's start with a simple example that shows how extension functions work and how they can improve the life of a developer using Weka. 

===================
Filtering Instances
===================

A common use-case is to filter a dataset. E.g. removing 33.0 % of a given dataset can be done like this in Java:

.. code-block:: Java

    // Get data
    Instances irisData = getIris();

    // Create filter for train set
    RemovePercentage rpf = new RemovePercentage();
    rpf.setPercentage(33.0);
    rpf.setInputFormat(irisData);
    
    // Apply filter
    Instances irisFilterd = Filter.use(irisData, rpf)

The same functionality can be achieved with Weka-Kt's `Instances.filter <https://steven-lang.github.io/weka-kt/com.github.stevenlang.wekakt.extensions/weka.core.-instances/filter.html>`_ extensions:

.. code-block:: Kotlin
    
    // Get data
    val irisData = getIris()
    
    // Filter data
    val irisFiltered = irisData.filter(RemovePercentage()) {
        percentage = 33.0
    }

which calls the following extension function on the :code:`irisData` object:

.. code-block:: Kotlin

    fun <T : Filter> Instances.filter(filter: T, body: T.() -> Unit): Instances {
        filter.body()                 // <body> is treated as function of all Filter subclasses <T>
        filter.setInputFormat(this)   // <this> refers to the receiving Instances object
        return Filter.useFilter(this, filter)
    }
    
The above defined function *extends* the Weka :code:`Instances` class. Therefore, the function acts as being part of the :code:`Instances` class and :code:`this` refers to the receiving :code:`Instances` object. The parameter :code:`body` is an extension function on the Filter subclass :code:`T` and can thus be called on the :code:`filter` object in the first line.

==========================
Generating Train/Test Sets
==========================

Another use-case is to split a given dataset into two subsets, namely the train and the test-set. To obtain these two sets, the following Java code would be necessary:

.. code-block:: Java

    // Create filter for train set
    RemovePercentage removePercentageTrain = new RemovePercentage();
    removePercentageTrain.setPercentage(33.0);
    removePercentageTrain.setInputFormat(irisData);

    // Create filter for test set
    RemovePercentage removePercentageTest = new RemovePercentage();
    removePercentageTest.setPercentage(33.0);
    removePercentageTest.setInvertSelection(true);
    removePercentageTest.setInputFormat(irisData);

    // User filters and generate train/test sets
    Instances train = Filter.useFilter(irisData, removePercentageTrain);
    Instances test = Filter.useFilter(irisData, removePercentageTest);
    

Using Weka-Kt's `Instances.split <https://steven-lang.github.io/weka-kt/com.github.stevenlang.wekakt.extensions/weka.core.-instances/split.html>`_, this can be reduced to:

.. code-block:: Kotlin
    
    // Split data using destructuring declaration
    val (train, test) = irisData.split(testPercentage = 33.0)
     
======================
Numpy-Like Data Access
======================
Numpy provides an easy and intuitive way of accessing data in a numpy array by passing indices to the square-brackets operator :code:`data[..]`. Kotlin's operator overloading allows us to extend the `Instances` functionality and provide definitions for numpy-like data access as shown in the following example:

.. code-block:: Kotlin

   // Get row
   val row = irisData[5]

   // Get value
   val valueByIndex = irisData[5, 3]

   // Get value by attribute
   val valueByAttribute = row[attribute]

   // Set row
   irisData[6] = row

   // Set value at index (6,3)
   irisData[6, 3] = 100.0


Furthermore, we can use Kotlins `Range` objects, e.g. :code:`2..5` (5 inklusive) or :code:`2 until 5` (5 exclusive), to access a slice of a dataset. Combining this with the brackets-operator results in even more flexible and shorter ways to access subset of a dataset:

.. code-block:: Kotlin

    // Get rows 2-20
    val rowSubset = iris[2..20]

    // Get rows 2-20 (explicit attribute selection with <ALL>)
    val rowSubsetEq = iris[2..20, ALL]

    // Get all rows and only columns 1-2
    val attributeSubset = iris[ALL, 1..2]

    // Get rows 2-20 and columns 1-2
    val subset = iris[2..20, 1..2]


More Weka Java vs Kotlin examples can be found `here <https://github.com/steven-lang/weka-kt>`_. All extension functions are well documented and demonstrated with code snippets in the `Weka-Kt documentation <https://steven-lang.github.io/weka-kt/>`_. Further ideas for Weka extension or improvements can be submitted as issues or pull requests at https://github.com/steven-lang/weka-kt.
