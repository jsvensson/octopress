---
layout: post
title: "A Ruby environment"
date: 2012-09-23 23:54
categories:
 - ruby
 - programming
comments: false
published: true
---

My Ruby environment on my Macbook was a mess. I wanted to get these notes jotted down mostly for my own advantage for the next time I need to do something like this.

<!-- more -->

## What a mess ##

I have been using [RVM](https://rvm.io/) for a fair while. It did _work_ (note those italics with an implied frown), but I didn't really need the features. I didn't need to run multiple versions of Ruby for various kinds of projects -- I only needed a fresher version than the Ruby 1.8.7 that OS X shipped with.

It got messy, and eventually things broke. I decided it was time to switch to [rbenv][rbenv].

## rbenv to the rescue ##

[rbenv][rbenv], you say? Let's get introduced.

{% blockquote %}
rbenv lets you easily switch between multiple versions of Ruby. It's simple, unobtrusive, and follows the UNIX tradition of single-purpose tools that do one thing well.
{% endblockquote %}

That sounds more to my liking -- no moving parts where they aren't needed. Rather than overriding the `cd` command and automagically running code when switching to directiories like RVM does, rbenv simply injects a few [shims][shims] and lets that override the Ruby version used. And all I need is that one fresh version.

## Installation ##

Installation under OS X couldn't get any easier thanks to [Homebrew](http://mxcl.github.com/homebrew/).

    brew update
    brew install rbenv
    brew install ruby-build

`rbenv` is the main package; `ruby-build` is a plugin that makes it a snap to download, compile and install some version of Ruby.

Once that's installed, you'll need to run two lines in your shell profile of choice. I have them [in my dotfiles][dotfiles], but stick them wherever (`~/.profile` might be an idea to always load it):

    export PATH="$HOME/.rbenv/bin:$PATH"
    eval "$(rbenv init -)"

The first line adds `rbenv` to your `$PATH` so it's available for use; the second loads the shims and some other stuff. There's [a detailed explanation][neckbeard] for all the gruesome details.

Now, reload/restart your shell and everything should be up and running. However, you still have to download an actual Ruby to give `rbenv` something to do.

As of writing this, `1.9.3-p194` is the current stable version. To download and install that:

    rbenv install 1.9.3-p194

Couldn't be any easier. You have some options ([detailed here][usage]) for how to run it, but as mentioned I simply want a fresh Ruby version. So I set it to run globally:

    echo '1.9.3-p194' > ~/.rbenv/version

Reload the shell again (just in case; might not be needed) and then check the Ruby version.

    % ruby -v
    ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-darwin11.4.2]

And there you have it.

## A bundle of gems ##

After that, time to configure [Bundler][bundler]. With RVM I ended up simply installing every single gem straight into the Ruby version I had active. Now I'm using Bundler instead to install gems per project in the project's directory. It results in a small bit of duplication, but it feels more manageable than cleaning out unused gems from your main Ruby install.

`rbenv-bundler` is a plugin that makes `rbenv` Bundler-aware and saves you from having to type `bundler exec` before everything you want to do with Bundler. As usual, it's available via Homebrew. 

    brew install rbenv-bundler

Bundler is the only gem you will have to install the traditional way, and you'll need to do it once for each version of Ruby you install.

    gem install bundler
    rbenv rehash

The second line is required after installing a new Ruby version, or installing any gem that adds new binaries, in order to update the shims.

Stick this in `~/.bundle/config` to prevent Bundler from sharing gems, and install gems in a `vendor` directory under your project root where the `Gemfile` resides.

    ---
    BUNDLE_PATH: vendor
    BUNDLE_DISABLE_SHARED_GEMS: "1"

And that's your Ruby environment set up and configured using `rbenv`. Just remember to run `rbenv rehash` after every `bundle install` in order to update the shims.

## Cleanup ##

There's one final step you can take to clean up any gems you may have installed into your system Ruby. To find them, run:

    /usr/bin/gem list --no-versions

After double-checking that list to make sure there's nothing you need (if you ever go back to running the system Ruby), you can uninstall them all:

    /usr/bin/gem list --no-versions | xargs sudo /usr/bin/gem uninstall


[neckbeard]: https://github.com/sstephenson/rbenv#section_2.3
[rbenv]: https://github.com/sstephenson/rbenv
[shims]: http://en.wikipedia.org/wiki/Shim_(computing)
[dotfiles]: https://github.com/jsvensson/dotfiles
[usage]: https://github.com/sstephenson/rbenv#section_3
[bundler]: http://gembundler.com/