# weka-blog

Code for the Nikola-based Weka blog.

The blog itself is located here:

[waikato.github.io/weka-blog/](https://waikato.github.io/weka-blog/)


## Installation

See the [Get started](https://getnikola.com/getting-started.html)
page on the Nikola homepage.


## Usage

Configuration file for the site is ``conf.py``.

To build the site:

```
nikola build
```

To see it:

```
nikola serve -b
```

To check all available commands:

```
nikola help
```

## Deployment to github pages

Happens via [.github/workflows/main.yml](.github/workflows/main.yml) 
action automatically, but can be manually performed as well:

* requires `ghp-import` package
* command for deploying

    ```
    nikola github_deploy
    ```

## Blogging

* posts go into the `posts` directory
* to make it easier to find posts, prefix them with the date (`yyyy-MM-dd-`)
* posts are written in reStructered Text
* structure of a post

  * [meta-data](https://www.getnikola.com/handbook.html#metadata-fields)

    ```
    .. title: Advanced Weka MOOC started
    .. slug: 2018-05-07-advancedmoocstarted
    .. date: 2018-05-07 08:00:00 UTC+12:00
    .. tags: mooc
    .. author: FracPete
    .. description:
    .. category: mooc
    ```

    * `title` - is the human readable title of the post
    * `slug` - is the URL to be used for this post
    * `date` - the date/time of the post (incl timezone)
    * `tags`- comma-separated list (`tag1, tag2, ...`)
    * `author` - the author of the blog post
    * `description` - leave empty
    * `category` - post category, eg `mooc` or `release`

  * empty line
  * actual content

* to avoid really long posts to clutter RSS feed and starting page, 
  add the following special command after a few paragraphs:

  ```
  .. TEASER_END
  ```

## Tools

Here are some tools:

* Online editors

  * [Online editor](http://rst.ninjs.org/)

* Command-line tools

  * [pandoc](https://pandoc.org/)
  * [rst2html](http://docutils.sourceforge.net/docs/user/tools.html#rst2html-py)

* GUI editors

  * [ReText](https://github.com/retext-project/retext)


## Help

Links are a bit different in reStructured Text and it is easiest to simply
use *anonymous* links:

```
`link text <link url>`__
```

Useful links:

* [Nikola handbook](https://www.getnikola.com/handbook.html)
* [A ReStructuredText Primer](http://docutils.sourceforge.net/docs/user/rst/quickstart.html)
* [Quick reStructuredText](http://docutils.sourceforge.net/docs/user/rst/quickref.html)
* [reStructuredText Directives](http://docutils.sourceforge.net/docs/ref/rst/directives.html)

