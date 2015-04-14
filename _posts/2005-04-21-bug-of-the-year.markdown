---
layout: post
title: "Bug of the Year"
date: 2005-04-21 10:14
comments: true
categories: 
---

I discovered the weirdest bug the other day...

I had built a new version of the imaging system, tested it and was ready to deploy it.  It was working great on the development box, and it seemed ready to go.  I downloaded the new version on the target machine, and fired it up, expecting to have only a few minutes downtime. And then...
<!--more-->

    Floating point exception

Oops.

But it wasn't my fault, honest!  Actually my first inclination was to blame one of the new packages I had installed.  One of the new features was to plot some statistics, and I was ready to put it down to a conflict or some weirdness with the new plotting package.  I checked everything three ways, and it was the right version, had all the right dependencies...  It had to be in my code - but where?

I cranked up the debug level in all modules (and was very glad at this point that I had decided to use the logging framework liberally, and with some granularity, and leave it in the production system).  I re-ran it and it *seemed* like the code was failing in a drawing routine for a custom widget.  This was extra weird, as I hadn't changed that module for a while, so it was the same as was running perfectly before.

I then went in and resorted to the age-old technique of adding `print` commands at various stages of the code, to see how far it was getting.  Sure enough, in the middle of the `on_draw()` method, in amongst some OpenGL calls, it was raising the `SIGFPE`.  I should mention at this point that this module was coded in Python, so any floating point errors would raise a real Python exception and display a nice stack trace showing me precisely where my error occured.  The fact that the error was the most generic message, obviously coming from the C runtime (and calling `abort()` rather rudely) made me wonder...

I stuck a premature `return` in the drawing code, and everything worked fine from then on.  But certain other widgets, also using OpenGL, could be made to trigger the error under certain circumstances.  Weirdness.  Time to fire up `gdb`.

I ran Python under the debugger, and ran the program (which also includes a bunch of custom Pyrex modules, which were not above suspicion).  The first thing that happens is a dump from a SIGFPE, but this was in the `i830_dri.so` module, in some `__driCreateScreen()` call.  I had actually noticed this once or twice before, but it had never caused a problem and so I had blissfully ignored it.  But now some alarm bells were going off.

I reverted the production machine to the previous release (thank goodness for Subversion and half-decent configuration management!) and went back to the office to track the problem down in detail on the test rig (which is identical to the production machine).

It turns out that the i830 video driver raises some floating point exceptions under certain OpenGL operations.  AFAICT this is a bug, but it hadn't caused a problem before, and hadn't been a problem on the dev box (which, as it turns out, uses the i810 driver which is why I hadn't noticed it before).  But why was it failing now and not before?  The plotting library `matplotlib` I had installed needs a linear algrabra package to run, and it can use either `Numeric` or `numarray`.  I chose the latter because it was newer.  It seems that numarray actually installs an FP signal handler, so when I added it to the mix, it was picking up on the driver FPEs as well.

After much binary partitioning (and a lot of hackery) I narrowed down the problem to the essentials, then wrote the most basic code that demonstrates the problem.  It is just a simple OpenGL program that displays a triangle, but if you so much as `import` the numarray module, you get a FPE in the drawing code.  Now I have to figure out where to report the bug!  (I suspect it is squarely the fault of the X i830 driver...)

The solution for the time being was to use `Numeric` for the linalg stuff, and I could put the plotting features back in.  This would give me time to chase up the driver bug.

The moral of this story: always test on a test rig that is the **same** as the production machine!

Oh, and also - logging is your friend!  It is definitely worth the extra few cycles to have granular, detailed logging available at the flick of a switch when things start going wrong.  Use a proper logging framework, not just `printf`, so you can log to a file, `syslog`, or whatever.
