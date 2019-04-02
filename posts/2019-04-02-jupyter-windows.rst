.. title: Jupyter notebooks (on Windows)
.. slug: 2019-04-02-jupyter-windows
.. date: 2019-04-02 13:44:00 UTC+13:00
.. tags: scripting
.. author: Peter Reutemann
.. description: 
.. category: scripting

In the previous `post <link://slug/2019-03-29-jupyter>`__, I quickly
described how to set up Jupyter Notebooks to run Java code. Now I want
to explain how to set things up on Windows using 
`Anaconda <https://www.anaconda.com/>`__. Of course, you have to have
a Java 9 JDK or later installed on your system, as the JDK's *JShell* 
feature is required to execute the Java code on the fly.

.. TEASER_END

* open a command prompt
* create a new environment using anaconda (e.g., for Python 3.5)

  ::

     conda create -n py35-ijava python=3.5

* activate environment 

  ::
     
     activate py35-ijava

* install Jupyter

  ::
  
     pip install jupyter

* download the latest IJava `release <https://github.com/SpencerPark/IJava/releases/>`__ (at time of writing, this was `1.20 <https://github.com/SpencerPark/IJava/releases/download/v1.2.0/ijava-1.2.0.zip>`__)
* unzip the IJava release (e.g., with your File browser or 7-Zip)
* change into the directory where you extracted the release, containing the `install.py`, e.g.:

  ::
  
     cd C:\Users\fracpete\Downloads\ijava-1.2.0

* install the kernel

  ::

     python install.py --sys-prefix

* start Jupyter

  ::

     jupyter-notebook

* now you can create new (Java) notebooks!

