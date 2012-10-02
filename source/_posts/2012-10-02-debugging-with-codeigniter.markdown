---
layout: post
title: "Debugging with CodeIgniter"
date: 2012-10-02 12:47
tags:
 - programming
 - php
comments: false
published: true
---

I've worked a lot with [CodeIgniter][codeigniter] lately, and debugging can be a bit of a pain due to how CodeIgniter works.

<!-- more -->

CodeIgniter throws the whole kitchen sink into a supermassive ~~black hole~~ object named `$this`, and anything CI-related is accessed via this object's methods. This makes debugging a bit of a pain since CI uses its own logging, and it's very lacking for anything but basic one-liner debug messages.

Enter [the dBug library][dbug]. It lets you dump a variable in a pretty table for inspection during page generation. However, it needs some gentle massage to get it and CodeIgniter to play along.

After downloading dBug, place `dBug.php` in `application/libraries`. Rename it to `Dbug.php` to match CodeIgniter's naming conventions.

Since dBug expects an argument in its constructor, we'll have to make a new empty constructor to use it in CodeIgniter.

While in there, rename the original constructor; I chose `show`. This is the method you'll call via the CI superobject to inspect things.

{% codeblock Modified constructors lang:php %}
// Empty constructor for CodeIgniter
function Dbug() { }

// Rename the original constructor
function show($var,$forceType="",$bCollapsed=false) {
  ...
{% endcodeblock %}

That's all the modifications needed. While in an development environment, it might be a good idea to auto-load dBug in `autoload.php`:

    $autoload['libraries'] = array('dbug');

Now you can easily inspect stuff:

    $this->dbug->show($this->session->all_userdata());


[codeigniter]: http://codeigniter.com/
[dbug]: http://dbug.ospinto.com/