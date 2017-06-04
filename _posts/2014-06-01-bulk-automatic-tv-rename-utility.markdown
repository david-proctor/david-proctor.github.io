---
layout: post
title:  "Bulk automatic TV rename utility"
date:   2014-06-01 14:01:04 -0800
categories: software batru front
featured-image: batru.png
---
Rename all your TV episode files to something reasonable with just a few clicks.<!--more-->

Have you ever found that the TV episodes on your hard drive are a mess? Perhaps they are arranged illogically and inconsistently. Perhaps their filenames are full of incomprehensible garbage that is of no use when you’re trying to find a specific episode. Perhaps you want to stream them between your devices, but some part of the interface only shows you a few characters at a time, and you need to wait for a slow scrolling text box to show you what episode you have selected. For you, and for whoever else wants to be rid of these problems once and for all, I have created Bulk Automatic TV Rename Utility.

This software automatically determines what TV show, season, and episode are contained in each file in a directory you specify, and allows you to edit them if it gets it wrong. With a few clicks, you set up how you want your files organized and renamed, and then sit back and let BATRU do the work for you. It’s free and open-source, released under the [GNU General Public License v3][gpl].

BATRU is open-source software that I created before getting my first job as a programmer as a sort of method of proving that I was capable of programming. Even though it's just a couple of years later as I write this, I already find the quality of the code in here quite poor, and am very embarassed at the lack of tests. Regardless, it works, so it remains available.

## Requirements

To run Bulk Automatic TV Rename Utility, you will need:

* Windows Vista or later
* [.NET Framework 4.5][dotnet]

Sadly, BATRU will not work on Mac or Linux.

## Download

[Download Bulk Automatic TV Rename Utility v1.0][download] (1.22MB)

## Source Code

The source is available on [BitBucket][source]. I should note that I originally wrote this as a demonstration of my programming skills in the hopes of getting my first job in the industry. Now that I've been programming as my career for a couple of years, there is a lot about this code that I frankly find embarassing now.

## Support

I don’t have any fancy support structure going for this software, so report any bugs on the [BitBucket project][source].

[gpl]: https://www.gnu.org/licenses/gpl-3.0.html
[dotnet]: http://www.microsoft.com/en-us/download/details.aspx?id=30653
[download]: http://davidproctor.io/wp-content/uploads/2014/06/Bulk-Automatic-TV-Rename-Utility1.zip
[source]: https://bitbucket.org/DavidProctor/bulk-automatic-tv-rename-utility