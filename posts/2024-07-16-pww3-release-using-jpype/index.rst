.. title: python-weka-wrapper3 release using JPype
.. slug: 2024-07-16-pww3-release-using-jpype
.. date: 2024-07-16 11:40:00 UTC+12:00
.. tags: github
.. author: FracPete
.. description:
.. category: code

Some of you might have come across the python-weka-wrapper3 library, which makes Weka available from Python:

  `https://github.com/fracpete/python-weka-wrapper3 <https://github.com/fracpete/python-weka-wrapper3>`__

When the project started out, it relied on the javabridge library in order to communicate with Java Virtual Machine
running the Weka process. Unfortunately, that project hasn't been maintained properly for a while now and it made
installation difficult or even impossible for M1/M2 Mac (ARM) users.

Over the last few weeks, I migrated the code base to using the `JPype library <https://github.com/jpype-project/jpype>`__
and updated the documentation accordingly.

Since the code-based appeared stable (tests and examples worked), I made a new release on Friday last week: **0.3.0**

There are several good news: **first**, the installation is much easier; **second**, ARM Mac users can use it
now as well; **third**, there were pretty much no interface changes necessary, as all changes happened behind
the scenes (only low-level access to Java objects changed).

So, if you had problems in the past, you might want to give it a go again.
