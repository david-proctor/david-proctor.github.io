---
layout: post
title:  "Got me: Read Only File System error when dual-booting Ubuntu and Windows"
date:   2018-06-16 14:18:00 -0800
categories: blog gotme
featured-image: gotcha-hibernate.jpg
---
I've been using Ubuntu on my home dev box for a few days and had a rude issue this morning: I couldn't manipulate any files on a secondary hard drive due to a "Read Only File System" error.<!--more-->

## The problem

For context, this system is running Ubuntu 18.04, dual-booted with Windows 10 (because, ridiculously enough, the new Wolfensteins and XCOMs aren't available on Linux). I discovered the problem while trying to work on this very web site:

{% highlight bash %}
$ bundle exec jekyll serve
  Configuration file: /media/david/Files/git/davidproctor.io/_config.yml
  Configuration file: /media/david/Files/git/davidproctor.io/_config.yml
              Source: /media/david/Files/git/davidproctor.io
         Destination: /media/david/Files/git/davidproctor.io/_site
   Incremental build: disabled. Enable with --incremental
        Generating...
  jekyll 3.4.0 | Error:  Read-only file system @ rb_sysopen - /media/david/Files/git/davidproctor.io/_site/category/art.html
{% endhighlight %}

Strangest of all, this problem emerged seemingly out of nowhere - I'd been happily working on the site in Ubuntu the day before. What could explain this "read-only file system" error?

## The cause

It is fairly well-documented that there are issues sharing a drive between Windows and Linux when Windows is hibernated. At the very least, I got a lot of results when I searched for this sort of issue. When I first encountered the problem, though, I searched terms including the error message and got a lot of results about using `fstab` to explicitly mount the drive and alter the permissions on it. This was a red herring.

The problem instead is that by default, Windows will 'hibernate' all of the drives in the system when it is shut down in order to speed up the next boot. When a drive is 'hibernated', Ubuntu treats it as read-only in order to avoid corrupting the data that Windows will need later. In this situation, we need to prevent Windows from hibernating when we shut down.

Although I haven't speicifically tested it, I imagine this is why the problem emerged suddenly - before, I'd been restarting the computer to boot Ubuntu, which didn't hibernate the drives. The problem emerged only when I started Ubuntu after shutting the system down.

## The Solution

Fortunately, the fix is easy: reboot in Windows, navigate to the Power Options, go to "Choose what the power options do", click "Change settings that are currently unavailable", and uncheck "Turn on fast startup."

![The Windows 10 'Choose what the power options do' settings dialog]({{ site.url }}/assets/gotcha-windows-power-settings.png)

After rebooting into Ubuntu my drives were available as expected, and I was able to access the drive and put together this post for you. all without recourse to any `fstab` nonsense.

> **Got Me** is a series wherein I risk making myself look like a dunce by writing about how I solved issues that I encountered that did not have readily-available solutions when I searched for them. May you save yourself time and heartache through this selfless act on my part.
