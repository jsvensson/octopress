---
layout: post
title: From the Ashes
date: 2012-09-17 15:39
comments: true
tags:
- life
- blogging
- textdrive
published: true
---

And with that rather pretentious title I try to do some blogging again. I look at the date on the last post, and... yikes, May 2010? About two and a half years ago.

<!-- more -->

When there's not much going on and you're in a (somewhat) bad place in your life there's really not much to write about. As I've been saying to mostly anyone that will listen: the danger of doing nothing for a long time is that it's really hard to stop doing nothing.

But I finally feel that things are in motion again, and more importantly that I actually have a say about the direction things are heading in. That's a very important part of actually feeling like you live your own life.

I hope I can write some more about that later. I have plenty of things I feel I want to get off my chest.

## Guts & Gears ##

I'm now running this blog on [Octopress](http://octopress.org/), a blogging framework based on [Jekyll](http://jekyllrb.com/). After many a year on [WordPress](http://wordpress.org/) I decided that a static page solution was more fitting for my needs -- there are zero security holes if there's absolutely nothing dynamic about it. Sometimes you don't need any moving parts.

WordPress is perfectly fine for blogging, but I started getting annoyed at seeing "new version available" every time I logged in (no, I don't log in very frequently) and felt I had to start the session by upgrading in case of security holes. And naturally I had to upgrade plugins as well.

The static site Jekyll generates is about as complicated as a hammer. If I feel I want something dynamic like comments, I can always enable an embedded third-party comment solution.

It feels good writing like this too. I have the blog entries as flat text files in a local Git repository (~~I'll toss it up on [GitHub](http://github.com/) later~~ [here it is](https://github.com/jsvensson/octopress)), I can run Jekyll in a local preview mode before I build a static site out of it and sync that to my web host.

I have everything from the old WordPress blog backed up, but I have to do some text massage on it before I can import that into Jekyll -- it's all written in Textile instead of Markdown, and I'll have to do some fiddling later to get that into Jekyll in a proper format. So at the moment it's very bare metal here.

## TextDrive Lives Again ##

There's also the whole issue with [Joyent][joyent] and [TextDrive][textdrive] that's partially to blame for me doing this switch.

Way back when in 2004, a fine gent named [Dean Allen][textism], having worked on a blog CMS named [TextPattern][textpattern] had the brilliant idea to start a web host aimed specifically at hosting for bloggers.

To get venture capital to start TextDrive, he had the idea to raise funds by offering 200 special investment spots. At 200 dollars each, that raised a total of 40,000 dollars to start the company. In return, these "VC200", as they ended up being called, were offered free hosting at TextDrive for the lifetime of the company.

I was number 74 out of these 200. As a poor student at the time, it was an excellent investment for me, given what web hosting _can_ cost.

It ended up lasting for eight years. TextDrive as a company had by now been bought by and merged with Joyent for several years.

And on August 16th 2012, Joyent suddenly announced the end of this lifetime offer. The message was very corporate. Very bureaucratic. As Dean later put it,

{% blockquote Dean Allen http://discuss.joyent.com/viewtopic.php?id=33821 Joyent forums %}
The announcement struck many as abrupt. Some took it to be an abandonment of, if not an insult to, your good faith, written in marketing and lawyer speak.
{% endblockquote %}

Naturally, nobody in the VC200 or other VC buy-in options (there were several others for other account services once the company was up and running) was very pleased. Not to mention boggled since the offer was for "the lifetime of the company", and Joyent didn't exactly look like they were in the red.

I understand Joyent's point of view. They have moved away from the kind of shared hosting TextDrive was based on, into cloud-based services. They are a whole lot bigger now than they were when they acquired TextDrive, and the shared hosting was an old legacy remnant. It makes perfect sense from a business point of view for them to get rid of an old legacy system that doesn't earn them a profit.

I was looking around for other options for hosting. Then I got a [message from Dean Allen][deanpost]. _TextDrive will live again_.

Eight years later, and Dean surprises me yet again. A new company for shared hosting, that will actually end up living in Joyent's cloud service, if I understand it correctly.

His post ends with a typical [textism](http://twitter.com/textism):

{% blockquote Dean Allen http://discuss.joyent.com/viewtopic.php?id=33821 Joyent forums %}
It gives me great pleasure to indicate that I’ll talk to you soon.
{% endblockquote %}

[deanpost]: http://discuss.joyent.com/viewtopic.php?id=33821
[joyent]: http://www.joyent.com/
[textdrive]: http://textdrive.com/
[textpattern]: http://textpattern.com/
[textism]: http://textism.com/