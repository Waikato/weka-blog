.. title: Making a Weka classifier
.. slug: 2018-10-08-making-a-weka-classifier
.. date: 2018-10-30 22:02:00 UTC+12:00
.. tags: github
.. author: eibe
.. description:
.. category: code

One role of the Weka software is to provide users with the opportunity to implement machine learning algorithms without having to deal with data import and evaluation issues: when a classifier has been written as a Java class that implements a couple of standard methods defined in the Weka framework, all the goodies that come with Weka are automatically applicable to it, and it will automatically show up in Weka's graphical user interfaces. To see what needs to be done, read on!

.. TEASER_END

To integrate your supervised learning algorithm into Weka, you need to make a Java class that implements Weka’s Classifier interface. Note that regression methods are also implemented as Classifier objects in Weka: the historic reason for this is that “regression” has sometimes been termed classification with “continuous classes” in early work on machine learning. 

There are three primary methods in this Classifier interface: buildClassifier(Instances), which will build the classification or regression model based on the given training instances, classifyInstance(Instance), which takes a test instance and returns a single predicted class value for the instance that is supplied, and distributionForInstance(Instance), which returns a class probability distribution instead, assuming the class attribute is nominal. The fourth method in the Classifier interface, getCapabilities(), returns the capabilities of the classifier, specifying what kind of data it can be applied to.

In practice, it is normally best to just extend Weka’s AbstractClassifier class, which implements this Classifier interface and is the superclass of pretty much all Classifier classes in Weka. It has default implementations for the distributionForInstance(Instance) and classifyInstance(Instance) methods specified in the Classifier interface. In your Classifier class, you *must* override at least one of these methods. You also need to implement buildClassifier(Instances) to actually build the classification/regression model. The AbstractClassifier also has a default implementation of the getCapabilities() method, which simply returns all possible capabilities, implying that the classifier can be applied to any kind of training data. Hence, implementation of this method is optional if you extend AbstractClassifier.

So, at a minimum, the Java class for your learning algorithm, assuming it extends AbstractClassifier, needs to implement two methods: buildClassifier(Instances) and distributionForInstance(Instance) (or classifyInstance(Instance)). The distributionForInstance(Instance) method must return a double array containing the estimated class probabilities for the different class values of the test instance if the class is nominal. If the class is numeric, it must return a single-element array with the numeric prediction for the test instance. The classifyInstance(Instance) method, if you choose to implement it instead of distributionForInstance(Instance), must return the index of the predicted class value (coded as a double) if the class is nominal and simply return the predicted class value if the class is numeric.

For Weka to find your class using its automatic Java class discovery mechanism when you actually want to run your Classifier through Weka’s graphical user interfaces (GUIs), your class needs to be in the Java CLASSPATH and in one of the standard classifier Java packages (e.g., weka.classifiers.functions or weka.classifiers.trees). If that is the case, regardless of where your class is physically located on your file system, it will show up in Weka’s GUIs automatically (for example, if you invoke the main() method of weka.gui.GUIChooser, which is the main entry point into Weka’s GUIs). 

Note that the standard Java package structuring rules apply: the directory structure for your class needs to match up with the fully qualified Java class name, e.g., weka.classifiers.functions.MyFunctionalClassifier must be located in a folder called “functions”, which is in turn located inside a folder called “classifiers”, which is in turn located in a folder called “weka”. 

Note also that, fFor Weka’s GUIs to work properly with your class, it needs to implement Java's Serializable indicator interface. AbstractClassifier does that and also implements a bunch of other interfaces, including the OptionHandler interface that is used for command-line option handling, so you normally will not need to worry about this. 

There are four command-line options already implemented in AbstractClassifier:

-output-debug-info
-do-not-check-capabiliities
-num-decimal-places
-batch-size

The first option will simply set the member variable m_Debug to true. You can use it in your class to output optional debug information (or you can just ignore it). The second option is only relevant if your class implements handling of capabilities. More on that in a second. The third option sets the value of the m_numDecimalPlaces variable. This should be used in the toString() method of your class, which you need to implement if you want a textual description of your model to be output by Weka, to specify the number of significant digits that are used when numbers are included in the output. The fourth option is ignored by almost all classifiers in Weka: it can be used to set a desired batch size for batch prediction when the classifier is used in batch prediction mode.

Below, I have included an implementation of a basic K-nearest-neighbours classifier for Weka. It implements buildClassifier(Instances) and distributionForInstance(Instance), the two required methods for a Classifier, but also illustrates option handling for setting the parameter K: the setK(int) and getK() methods are used for setting K in Weka’s GUIs. Whenever a matching getter and setter method pair is found in a Classifier object, it will automatically show up in the GenericObjectEditor used to configure learning schemes in Weka’s GUIs. Looking at the code a bit further, the information in the @OptionMetaData(…) tag is used to specify the corresponding command-line option for the setter/getter pair. The toString() method in this example code is rudimentary and just outputs the number of neighbours used by the classifier.

``
/**
* This code is released to the public domain. Use as you see fit.
*/
package weka.classifiers.lazy;

import weka.core.Instance;
import weka.core.Instances;
import weka.classifiers.AbstractClassifier;
import weka.core.neighboursearch.LinearNNSearch;
import weka.core.neighboursearch.NearestNeighbourSearch;
import weka.core.OptionMetadata;
import weka.core.Capabilities;

/**
* Implements the k-nearest-neighbours method for classification and
* regression.  Existing WEKA code is used to retrieve the K nearest
* neighbours for a test instance. The number of neighbours to use is
* a parameter that the user can specify, via a get...()/set...()
* method pair for WEKA's GUIs and a Java annotation for command-line
* option handling.
*/
public class KNN extends AbstractClassifier {

   /** The number of neighbours to use */
   protected int m_K = 1;

   /** The method to be used to search for nearest neighbours. */
   protected NearestNeighbourSearch m_NNSearch = new LinearNNSearch();

   /**
    * Returns capabilities of the classifier.
    *
    * @return the capabilities of this classifier
    */
   public Capabilities getCapabilities() {
       Capabilities result = super.getCapabilities();
       result.disableAll();

       // predictor attributes
       result.enable(Capabilities.Capability.NOMINAL_ATTRIBUTES);
       result.enable(Capabilities.Capability.NUMERIC_ATTRIBUTES);
       result.enable(Capabilities.Capability.DATE_ATTRIBUTES);
       result.enable(Capabilities.Capability.MISSING_VALUES);

       // class
       result.enable(Capabilities.Capability.NOMINAL_CLASS);
       result.enable(Capabilities.Capability.NUMERIC_CLASS);
       result.enable(Capabilities.Capability.DATE_CLASS);
       result.enable(Capabilities.Capability.MISSING_CLASS_VALUES);

       return result;
   }

   /**
    * Method to set the number of neighbours. Including metadata annotation
    * to implement command-line option handling for this parameter.
    */
   @OptionMetadata(displayName = "number of neighbours", description = "Number of neighbours to use (default = 1).", 
                   commandLineParamName = "K", commandLineParamSynopsis = "-K <int>", displayOrder = 1)
   public void setK(int k) {
       m_K = k;
   }

   /** 
    * Method to get the currently set number of neighbours.
    */
   public int getK() {
       return m_K;
   }

   /**
    * Initialises the classifier from the given training instances.
    */
   public void buildClassifier(Instances trainingData) throws Exception {

       // Can the classifier handle the data?
       getCapabilities().testWithFail(trainingData);

       // Make a copy of data and delete instances with a missing class value
       trainingData = new Instances(trainingData);
       trainingData.deleteWithMissingClass();

       // Trivial for KNN: just initialise NN search class
       m_NNSearch.setInstances(trainingData);
   }

   /**
    * Returns class probability distribution (classification) or numeric
    * target value (regression) for a given test instance.
    */
   public double[] distributionForInstance(Instance testInstance) throws Exception {

       // Add instance to NN search so that attribute ranges can be updated
       m_NNSearch.addInstanceInfo(testInstance);

       // Get the list of neighbours
       Instances neighbours = m_NNSearch.kNearestNeighbours(testInstance, m_K);

       // Calculate calculate class probability distribution or target value
       double[] dist = new double[testInstance.numClasses()];
       for (Instance neighbour : neighbours) {
           if (testInstance.classAttribute().isNominal()) {
               dist[(int)neighbour.classValue()] += 1.0 / neighbours.numInstances();
           } else {
               dist[0] += neighbour.classValue() / neighbours.numInstances();
           }
       }
       return dist;
   }

   /**
    * Returns a textual description of the classifier.
    */
   public String toString() {

       // Not much to output here for KNN: no explicit model
       return "KNN with " + m_K + " neighbours";
   }

   /**
    * Main method, can be used to run classifier from command-line.
    */
   public static void main(String[] args) {
       runClassifier(new KNN(), args);
   }
}
``

In this simple classifier, the biggest method is the getCapabilities() method. This method is optional. It specifies what kind of data this classifier is able to deal with and is used in Weka’s GUIs to grey out a classifier if it is not applicable to a particular dataset. It is also used in the buildClassifier(Instances) method in this example code: getCapabilities().testWithFail(trainingData) will use this method to check whether the classifier is actually applicable to the data provided as the training data. Note that implementing this method is really optional: AbstractClassifier has a default implementation of getCapabilities() that does not restrict the classifier in any way. Basically, getCapabilities() only needs to be implemented if you want your classifier to be used by other users, to make application of your classifier more user friendly.

The main() method in the example class is used to run the classifier from the command-line. The runClassifier(Classifier, String[]) method called in this main() method will use Weka’s Evaluation class to enable a cross-validation, etc., of the classifier on the data that is provided. It will automatically enable all the general command-line options available for evaluation of Weka classifiers and also make use of the command-line options specific to the classifier that are provided in the code for the classifier via the @OptionMetaData tag. Note that if you only want your classifier to be used in Weka’s GUIs, you do not need the main() method and you do not need the @OptionMetadata annotation either.

Below, I have also included a minimalist version of the example class that has the absolute minimum amount of code necessary to use the classifier in Weka’s GUIs. It would be enough to run experiments with the K-nearest-neighbour method in Weka’s Experimenter GUI, etc. As you can see, it is pretty straightforward to implement a classifier in Weka, particularly if you only want to quickly run some experiments with a learning algorithm that you have dreamed up!

``
package weka.classifiers.lazy;

import weka.core.Instance;
import weka.core.Instances;
import weka.classifiers.AbstractClassifier;
import weka.core.neighboursearch.LinearNNSearch;
import weka.core.neighboursearch.NearestNeighbourSearch;

public class KNNMinimal extends AbstractClassifier {

   protected int m_K = 1;

   protected NearestNeighbourSearch m_NNSearch = new LinearNNSearch();

   public void setK(int k) {
       m_K = k;
   }

   public int getK() {
       return m_K;
   }

   public void buildClassifier(Instances trainingData) throws Exception {

       trainingData = new Instances(trainingData);
       trainingData.deleteWithMissingClass();

       m_NNSearch.setInstances(trainingData);
   }

   public double[] distributionForInstance(Instance testInstance) throws Exception {

       m_NNSearch.addInstanceInfo(testInstance);

       Instances neighbours = m_NNSearch.kNearestNeighbours(testInstance, m_K);

       double[] dist = new double[testInstance.numClasses()];
       for (Instance neighbour : neighbours) {
           if (testInstance.classAttribute().isNominal()) {
               dist[(int)neighbour.classValue()] += 1.0 / neighbours.numInstances();
           } else {
               dist[0] += neighbour.classValue() / neighbours.numInstances();
           }
       }
       return dist;
   }
}
``

One more thing: if you want your class to be located in a new Java package that is not one of Weka’s standard packages for classifiers, you will need to make an appropriate version of the GenericPropertiesCreate.props file for Weka. For example, the RPlugin package for Weka defines a new weka.classifiers.mlr package and has the following info in the GenericPropertiesCreator.props file:

``
weka.classifiers.Classifier=\
weka.classifiers.mlr
``

That is it for me for today. Hope you found this useful.