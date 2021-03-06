.. title: Weka Debian packages
.. slug: 2019-09-23-weka-debian-packages
.. date: 2019-09-23 13:21:00 UTC+12:00
.. tags: github
.. author: FracPete
.. description:
.. category: snapshots

Users installing Weka on Linux must have always felt left out
a bit, with no installer available, instead having to deal with
just a ZIP file. For power users, that would not have mattered,
but users new to Linux may have found that a bit more challenging.

Well, things have changed - at least for users of Debian or
one of its many derivatives like Ubuntu - with the advent
of `snapshots <https://www.cs.waikato.ac.nz/~ml/weka/snapshots/weka_snapshots.html>`__ 
being available for download as `Debian packages <https://en.wikipedia.org/wiki/Deb_%28file_format%29>`__ (stable 3.8 and developer version).

.. TEASER_END

Downloading and installing
==========================

All snapshots, ZIP files and Debian packages, get generated by our 
build server and are available from the following URL:

`cs.waikato.ac.nz/~ml/weka/snapshots/weka_snapshots.html <https://www.cs.waikato.ac.nz/~ml/weka/snapshots/weka_snapshots.html>`__

Simply download the version that you would like to install on your
system, stable or developer.

Once downloaded, you can either install the package from the
command-line using *sudo dpkg -i filename.deb* or double-click it
to use a graphical installer like `GDebi <https://itsfoss.com/gdebi-default-ubuntu-software-center/>`__ 
or *Ubuntu's Software Center* (for some reason the *Software 
Center* thinks that Weka is proprietary).


Starting Weka
=============

Depending on what version you installed, you will have two scripts
in your */usr/bin* directory:

::

    weka-stable-gui
    weka-stable-run

Or:

::

    weka-dev-gui
    weka-dev-run


GUI
---

The *-gui* script will start Weka's GUIChooser with 512MB of heap space by default.
You can increase the heap with the *-memory* option, e.g., *-memory 2g* to use 2GB.
If you want to execute another GUI class, e.g., the Explorer, then you can supply
this with the *-main* option, e.g., *-main weka.gui.explorer.Explorer*. 

Starting the Explorer of the developer version with 2GB of heap from the
command-line would look like this:

::

    weka-dev-gui -main weka.gui.explorer.Explorer -memory 2g


The Debian package also installs a *.desktop* file in */usr/share/applications*,
which simply calls the *weka-{dev|stable}-gui* script and should show up in your
applications menu automatically.


Run
---

The *-run* script executes Weka's *weka.Run* class, for executing classes, like
classifiers, clusterers, filters, etc. from the command-line. The *weka.Run* tool
allows you to specify shortened classnames (as long as they are unique), e.g., 
*.J48* instead *weka.classifiers.trees.J48*. This script also offers the *-memory*
option to chage the heap size from its default 512MB.

The following command-line cross-validates J48 (stable version) on the labor
UCI dataset:

::

    weka-stable-run .J48 -t /some/where/labor.arff


Building packages
=================

This section is only relevant to people wanting to build their own
Debian packages, e.g., when including their own classes in a custom
build, or people that want to simply know how it is done.

Building of Debian packages is only possible on Linux
systems that have the Debian package tools installed (*fakeroot*, *dpkg-deb*),
as the build process uses the `Debian Maven Plugin <https://github.com/fracpete/debian-maven-plugin>`__ to generate them, which in turn relies on these system tools.

There are two different build profiles available, one
that uses the `Maven <https://github.com/fracpete/debian-maven-plugin>`__ 
build framework to collect all the required
artifacts (*mvn-deb-pkg*) and one that reuses jar files built
previously by `ant <http://ant.apache.org/>`__ (*ant-deb-pkg*).
Our build server uses the latter one.

Using Maven
-----------

You can use the following command-line for building a Debian package
(located in your *target* directory):

:: 

    mvn clean install -DskipTests=true deb:package -P mvn-deb-pkg
   


Using Ant
---------

Since the Ant profile relies on previously generated jar files, you
need to execute two commands to successfully build a Debian package
in your *target* directory:

::

    ant clean exejar srcjar
    mvn deb:package -P ant-deb-pkg

