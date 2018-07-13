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

* requires `ghp-import` package
* command for deploying

    ```
    nikola github_deploy
    ```


## jenkins on CentOS

CentOS does not have Python 3.5 packages directly available through packages,
but rather through *software collections* (`scl`). 

Python 3.5 is enabled like this (from an interactive shell):

```bash
/bin/scl enable rh-python35 bash
```

Assuming that the weka-blog repo is installed in `/var/lib/jenkins/repos/weka-blog`, 
you can use the *Execute shell* build step with this script to automatic deploy
the github pages *if* the repo has changed:

```bash
cd /var/lib/jenkins/repos/weka-blog
COUNT=`git pull | grep -v "Already up-to-date" | wc -l`
if [ $COUNT -gt 0 ]
then 
  /bin/scl enable rh-python35 - << \EOF
cd /var/lib/jenkins/repos/weka-blog
/var/lib/jenkins/repos/weka-blog/venv/bin/nikola github_deploy
EOF
fi
```

For getting the jenkins user to deploy to github, you need to:

* create an ssh key as user jenkins

  ```bash
  ssh-keygen -t rsa -f ~/.ssh/github_wekablog
  ```

* add the following to `~/.ssh/config`

  ```
  Host github-wekablog
     HostName github.com
     User git
     IdentityFile ~/.ssh/github_wekablog
     IdentitiesOnly yes
  ```

* add the public ssh key under the repo's *Settings -> Deploy keys* 
  with *write* access

* update the `.git/config` file in the checked out repo, changing the 
  *remote origin* URL to this (using the just created identity):

  ```
  url = git@github-wekablog:Waikato/weka-blog.git
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

