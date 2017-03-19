---
layout: post
title: "Starting with jekyll"
description: ""
category: 
tags: jekyll, ruby, windows, mac os x
---
{% include JB/setup %}

A quick walthrough of the most basic commands for installing and "developing" with Jekyll.

<!--more-->

### Requirements

You need to have Ruby installed and configured properly on your system, and you need to be somewhat comfortable firing up a Terminal window, Windows CMD, Bash, or whatever flavor you prefer.

For installing and configuring Ruby on Windows, check my other post [Ruby on Windows]({% post_url 2017-03-18-ruby-on-windows %}), and follow the steps described, then come back to this post.

In case of Mac OS X, there're multiple ways; but by far the easiest, and the one I choose, is to simply install Apple Xcode, as this will also install a set of command line dev tools, Ruby being one of them.

### Installing Jekyll

Installing Jekyll is really super simple. Simply enter `gem install jekyll`, and wait for it to finish.

After this you might want to install a series of other gems, depending on how you want to build and package your site.

If you're writing a blog like this one, you might want to install <a href="http://octopress.org/" target="_blank">Octopress</a>, and for that, simply enter `gem install octopress`, and after that, bootstrap your install.

You might want to use <a href="http://bundler.io/" target="_blank">Bundler</a> as part of your deployment process, and for that, simply enter `gem install bundler`, and go from there.

I'm sure that by now you've gotten the hang of installing gems.

### Testing a Jekyll site

For testing your site locally, jekyll can compile it for your and serve it up for testing in a browser, using it's built-in lightweight, and fairly simple, http-stack.
To do this, simply open a shell, cd to the folder your project lives in, type `jekyll serve` and hit Enter.

Now, to prevent you from having to stopping and starting the process each time you change something, the debugging server has a watcher built-in. To utilize that, type `jekyll serve -w` instead, and hit Enter.
Now you'll see the process writing output to your Terminal window, each time you touch a file in your Jekyll directory. And the pages served are being updated accordingly.

Another useful CLI argument is `--drafts` which, you guessed it, includes the posts that currently sits in your `_drafts` folder, to see how your working progress looks like.
The full command for debugging will then be `jekyll serve -w --drafts`.

And that's it really, you now know the basics of Jekyll CLI and debugging. Naturally you can find way more eahaustive articles elsewhere, but this wasn't intended to be a complete tutorial, merely a quick start guide.