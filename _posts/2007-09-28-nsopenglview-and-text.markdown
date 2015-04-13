---
layout: post
title: "NSOpenGLView and text"
date: 2007-09-28 10:28
comments: true
categories: 
---

The Cocoa view class `NSOpenGLView`, which automates all the initialisation required to provide an OpenGL context for drawing, is very useful indeed.  It would also seem that `aglUseFont` is a nice simple way to load up a font to draw some text in your view.  So long as it isn't the aforementioned `NSOpenGLView`, that is.
<!--more-->

I found out the hard way that the way `NSOpenGLView` initialises the OpenGL context is not compatible with the `AGL` API.  So you can handle all the pixel formats and so on yourself but have easy fonts, or the other way round.  The `glut` text call does actually work, albeit in a rather less flexible manner.

I guess this explains why there's a bunch of font/text utility libraries for OpenGL - it's non-trivial.  But if all you want is a quick and dirty text-drawing function, go for glut.

One day I'll figure out how to properly render accelerated Quartz text into an OpenGL context, and write an article about it.
