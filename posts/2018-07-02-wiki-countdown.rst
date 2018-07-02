.. title: Wiki countdown
.. slug: 2018-07-02-wiki-countdown
.. date: 2018-07-02 14:12:00 UTC+12:00
.. tags: wiki
.. author: FracPete
.. description:
.. category: wiki

Earlier this year, the company hosting the `Weka Wiki <http://weka.wikispaces.com/>`__
made a notice available when logging into the wiki:

Notice
  "Over the last twelve months we have been carrying out a complete technical
  review of the infrastructure and software we use to serve Wikispaces users.
  As part of the review, it has become apparent that the required investment to
  bring the infrastructure and code in line with modern standards is very
  substantial. We have explored all possible options for keeping Wikispaces
  running but have had to conclude that it is no longer viable to continue to
  run the service in the long term. So, it is with no small degree of
  nostalgia, that we will begin to close down later this year."

With that in mind, we started looking for alternatives. In the end, we decided
to go with a semi-wiki approach by using `mkdocs <http://www.mkdocs.org/>`__
on code that is hosted on Github. The reasons were:

* community engagement is easy via `pull requests <https://help.github.com/articles/about-pull-requests/>`__
* mkdocs uses `markdown <https://daringfireball.net/projects/markdown/>`__ 
  for the articles, a markup language that many people are familiar with
* mkdocs can `automatically deploy <https://www.mkdocs.org/user-guide/deploying-your-docs/>`__ 
  to `Github Pages <https://pages.github.com/>`__

Downside to switching to markdown is that we have to manually migrate articles,
switching from one markup language to another. This means, it is a lengthy process
getting the new site full operational, so please bear with us.

So, long story short: the old wiki is dying on July 31st, the new one is being 
fleshed out here:

`waikato.github.io/weka-wiki <https://waikato.github.io/weka-wiki/>`__

