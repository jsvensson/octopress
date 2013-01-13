---
layout: post
title: "Keeping an Octopress Site Clean"
date: 2012-12-26 14:48
categories:
 - octopress 
comments: true
published: true
---

[Octopress][octopress] uses [rsync][rsync] to deploy to the web host. Keeping the files on the host cleaned up might be a bit of a worry if you're like me{% fn_ref 1 %}.

<!-- more -->

Imagine this scenario: you rename or delete{% fn_ref 2 %} an Octopress post and generate and deploy. Your site is updated to reflect the changes, but you will still have the old files left on the host, likely to receive incoming search engine hits.

The Octopress `Rakefile` has the `rsync_delete` option which will make rsync delete any file at the destination that doesn't exist in the source.

That's fine to use, unless you have non-Octopress files at the destination. I run my Octopress blog in the root of my site and have other directories there that I for natural reasons don't keep in my Octopress source directory, so `rsync --delete` would delete those.

<ins>Update Jan 8th 2013:</ins> After a brief "doh" moment I realized that Octopress already supports this, [as clearly documented](http://octopress.org/docs/deploying/rsync/). I was aware of the `rsync-exclude` file, but in my mind that meant it only dealt with not uploading the excluded local files to the destination. As clearly mentioned, if the `rsync_delete` option is true it will not delete files listed in `rsync-exclude` on the destination.

However, if you want to use the full abilities of rsync's filter rules, the rest of this article still stands and has been modified to accommodate better exclude filter rules.

## Rsync Filter Rules

I found the solution in rsync's [filter options][rsync-man]. I've used `--include` and `--exclude` often, but after some RTFMing I learned that those parameters are just a subset of the actual filter rules.

First of all, set it up{% fn_ref 3 %} in your `Rakefile`:

``` ruby
rsync_delete = true
rsync_args   = "--filter='merge rsync-filter'"
```

Next, create a file named `rsync-filter` in your Octopress root directory. This is where you state which files you want to keep untouched on the destination host.

    P dir1/
    P dir2/
    P file.html

There are plenty of other options for the filter, but `P` is what we're after here -- it makes the files and folders excluded, meaning rsync will leave them alone on the destination when `--delete` is used. The rsync man page says this about `--delete`:

> This  tells rsync to delete extraneous files from the receiving side (ones that aren’t on the sending side), but only for the directories that are being synchronized. (...) **Files that are  excluded from transfer are also excluded from being deleted** unless you use the `--delete-excluded` option or mark the rules as only matching on the sending side (...)

## Danger! High Voltage!

While testing this out, use `--dry-run` as well to check that it actually works as intended -- otherwise you might nuke your whole site! [That's not fun][highvoltage].

{% footnotes %}
  {% fn That is, slightly obsessive-compulsive. %}
  {% fn Which is something I would be very wary of doing because [cool URIs don't change](http://www.w3.org/Provider/Style/URI.html). %}
  {% fn You might not have this option in your `Rakefile` yet -- it's a [new addition](https://github.com/imathis/octopress/commit/48d3e75ff5d3468369ca8104379b870f7cf600b1) pushed by yours truly. Pull [a fresh Octopress](https://github.com/imathis/octopress) to get it. %}
{% endfootnotes %}

[octopress]: http://octopress.org/
[rsync]: http://en.wikipedia.org/wiki/Rsync
[rsync-man]: http://linuxcommand.org/man_pages/rsync1.html
[highvoltage]: http://www.youtube.com/watch?v=2a4gyJsY0mc
