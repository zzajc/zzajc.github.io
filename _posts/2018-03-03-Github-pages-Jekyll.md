---
layout: post
title: "Simplest Github pages with Jekyll"
date: 2018-03-03
tags: github pages Jekyll
author: Jiri Hanousek
---

I decided to write this post because I spend unnecessary hours of installing Jekyll and supporting stuff on Windows. 

The main message is:

**To manage your static webpage code with Jekyll on github pages you do not need to install anything to your computer.**

Now my simplified tutorial to Jekyll on github pages. 

1. Define your pages github repository with name conventions **YOUR_NAME**.github.io (follow ultra clear instructions on [github pages]{pages.github.com}
2. Clone the repository
3. Define most basic Jekyll folder structure with a few necessary files
    * / root
        * .git
        * _posts
            * YYYY-MM-DD-Title-Of-Your-Post.md
        * _config.yml  ([see my current version](https://github.com/zzajc/zzajc.github.io/blob/7dd3d766379967d9cfa08dcacefe8fb25d4776bf/_config.yml))
        * index.html  ([see my current version](https://github.com/zzajc/zzajc.github.io/blob/7dd3d766379967d9cfa08dcacefe8fb25d4776bf/index.html))
4. Create your posts and put them to *_posts* directory with correct naming convention
5. Push your changes
6. See result on web adress **YOUR_NAME**.github.io

For simplest start I only needed to setup two files
* **_config.yml** by filling few values and clearing/commenting some unnecessary ones.
* **index.html** which defines a template which is populated with list of posts which are present in *_posts* folder

After you push your changes wait a moment (few minutes max) and your site will be automatically rebuilt.

You are in and ready to start to modify what does to conform to your ideas. With this setup you will not be able to
see your site locally but for my simple purposes it is very satisfying just to push, wait and refresh.

