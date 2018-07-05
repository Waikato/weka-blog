.. title: Weka is now on Github
.. slug: 2018-05-30-weka-on-github
.. date: 2018-05-30 22:02:00 UTC+12:00
.. tags: github
.. author: FracPete
.. description:
.. category: code

In order to make Weka more accessible for people that like
having access to the latest code, we have been looking into 
ways of bringing Weka to Github. Especially, since our
faculty already has an organizational account `there <https://github.com/Waikato>`__.

However, Weka was original stored in a `CVS <https://en.wikipedia.org/wiki/Concurrent_Versions_System>`__
repository and then got migrated to `Subversion <https://en.wikipedia.org/wiki/Apache_Subversion>`__. Not only has it a lot of branches, not all of
them are publicly available, like commercial ones. This all
complicated the migration to Github.

.. TEASER_END

In the end, we decided to simply use Github as a mirror
of our inhouse Subversion repository. With that, you can
at least clone and fork in the usual Github fashion. The
only downside is, that you can't make any pull-requests,
being downstream. For fixes/additions, you still need to send
in a patch file to the `Weka mailing list <https://list.waikato.ac.nz/mailman/listinfo/wekalist>`__.

Long story short, we now have two repositories, one for 
developer version (*trunk* or *HEAD*) and one for the
stable 3.8 branch:

* `github.com/Waikato/weka-trunk <https://github.com/Waikato/weka-trunk>`__
* `github.com/Waikato/weka-3.8 <https://github.com/Waikato/weka-3.8>`__

Enjoy!
