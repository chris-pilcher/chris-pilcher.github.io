---
layout: post
title: "Using this jekyll blog theme"
date: 2015-07-31 02:22:13
categories: jekyll update
---
Installation
-------------

If you're starting a new blog, you want to clone-and-go:\\
Just `git clone git@github.com:rcrmn/jekyll-dusk-blog.git`, make any changes
you want to the template, pages or `_config.yml` and start blogging with Jekyll. Easy.


Usage
------

The usage described here assumes that the blog will be hosted in github pages
and that it will be a user gh-page, instead of a project gh-page. If you are
using<!--more--> it for the second case, you should be careful using the rake task provided
to publish the blog since it is intended to work for a user-level gh-page.

Since this blog theme uses some plug-ins not supported on github pages, the best
way to publish the blog is using the rake task provided.

You should work on the blog itself in the `jekyll` branch. The master branch
will be used as the compiled site branch.

The first thing you should do after cloning is go to the `_config.yml` file and
change it to suit your site. Configuration like the blog title, your social
networking info, or the blog navigation menu.

Once that is done, you should start writing content! You can create new posts
manually, or using the rake tasks:

- To create a new content post:

      rake new:post

- To create a new link post:

      rake new:link

These tasks will ask the title of the post to be inputted, and in the case of the
link task, it will ask for the link itself. They will create a new post in the
_posts directory containing the basic information. They are quite self-explanatory.


Deployment
-----------

Once you have the blog prepared to publish, from the jekyll branch you can call
rake to do that work for you:

    rake publish

will compile the site, commit it to the master branch and push it to the master
branch at origin. This uses the flag --force, so be careful with it.


Features
--------
- Desktop and mobile responsive layouts
- Configurable social links via _config.yml. Remove any you don't need.
- Customized menu-bar links via config.
- Code highlighting with `` `...` `` and liquid `{{ "{% highlight " }}%}` tags via pygments. See example posts. [List of language tags](http://pygments.org/docs/lexers/)
- Front-page arbitrary excerpts with tag `<!--more-->`


Demo
-----
You can find a demo here: <http://rcrmn.github.io/jekyll-dusk-blog/>

License
--------

MIT
