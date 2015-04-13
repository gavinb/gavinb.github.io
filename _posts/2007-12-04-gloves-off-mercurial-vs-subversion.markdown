---
layout: post
title: "Gloves off: Mercurial vs Subversion"
date: 2007-12-04 23:53
comments: true
categories: 
---

I use [Subversion](http://subversion.tigris.org/) on a daily basis, and [Mercurial](http://www.selenic.com/mercurial/) a few days a week.  I have noticed that Mercurial *seems* to be faster with a lot of common operations, but I figured it wasn't a fair comparison as Mercurial was always operating locally, while Subversion has to hit the network for many (but certainly not all) operations.

So I decided to do some very rough performance comparisons on a local repository.  And after some quick timings of common operations, this is what I found...
<!--more-->
(Updated: fixed table and added graph.)

## Method

This is a very rough, and thus not very rigourous evaluation.  I performed all tests on a PowerBook G4 1GHz running Mac OS X 10.5 under the same operating conditions (ie. on the train home from work).  The versions used:

- Subversion 1.4.4
- Mercurial 0.9.5 with Python 2.5

I created both repositories locally (under `/tmp`) and performed the equivalent operations with both VC systems.  The source tree used was a recent version of the Django source tree, which is fairly large (2263 files, 568 dirs, 18Mb).  For the `svn import` equivalent, I did an `hg commit -A` of the source tree.  For the `svn checkout` equivalent, I did an `hg clone` of the tree.  For the non-mutating operations (eg. `status`) I ran the same command 5 times and picked the best.

I tried to be as fair as possible, but not take days to actually come up with some figures.  So without further ado...

## Results

All times as reported by the `bash time` command, taking the real time value (ie. elapsed system time):

<table>
<tr><th>Operation</th><th>Mercurial</th><th>Subversion</th><th>Speedup</th></tr>
<tr><td>Create Repository</td><td>0.346s</td><td>0.279s</td><td>svn 1.2X</td></tr>
<tr><td>Import source tree</td><td>12.241s</td><td>1m3.781s</td><td>hg 5.2X</td></tr>
<tr><td>Checkout</td><td>19.808s</td><td>1m1.356s</td><td>hg 3.1X</td></tr>
<tr><td>Status (1 mod)</td><td>0.631s</td><td>0.265s</td><td>svn 2.4X</td></tr>
<tr><td>Diff (1 mod)</td><td>0.778s</td><td>0.212s</td><td>svn 3.7X</td></tr>
<tr><td>Diff (2 mods)</td><td>0.784s</td><td>0.331s</td><td>svn 2.4X</td></tr>
<tr><td>Commit (2 mods)</td><td>1.164s</td><td>3.442s</td><td>hg 3.0X</td></tr>
<tr><td>Add file</td><td>0.544s</td><td>0.121s</td><td>svn 4.5X</td></tr>
<tr><td>Commit</td><td>0.922s</td><td>2.920s</td><td>hg 3.2X</td></tr>
<tr><td>Total</td><td>37.218s</td><td>132.707s</td><td>hg 3.6X</td></tr>
</table>

![Mercurial Subversion Performance](http://skitch.com/gavinb/rpts/mercurialsubversionperformance)

## Conclusions

A simple count of which is faster for each operation shows both scored 5/10, so in that sense they are evenly matched.  But in the cases where Subversion was faster, the times were still well under 1sec, so the difference is far less noticeable.  It is clear from the above that for many expensive operations, Mercurial is significantly faster than Subversion, even for large I/O operations such as commits and importing.  Given Mercurial is written in Python (apart from one small module IIRC), and Subversion is compiled C, this is all the more impressive.  Looking at the overall total times for the test workload, Mercurial comes out at 3.6 times faster than Subversion.  If you are spending a lot of time with your VC system, this can make a huge difference to your workday productivity.

Clearly the build-in diff engine in Subversion gives it a major advantage in diffs, a very important and common operation. Mercurial is still competitive, and in real-world tests seem to scale well.  Having tested it at work with an even larger source tree (the Boost++ library), Mercurial actually seemed to be faster than Subversion for status operations in particular (around a second versus several seconds).

It should be noted that since running Subversion from a local repository like this is the exception rather than the norm for most projects, the performance advantage it has thanks to its fast diff engine evaporates against network latency.  Thus using a distributed VCS such as Mercurial can be a great productivity boost, as you only pay for the network when you need to branch or merge between developers, a far less common operation.

This comparison has reinforced my positive impressions of Mercurial, and I hope it serves to bolster peoples' confidence in its ability to scale and perform.  It has all the features of Subversion, plus many more groovy features like patch queues (which I may blog about one day).  Since Mercurial is less well known, I hope more people will [give Mercurial a try](http://www.selenic.com/mercurial/).
