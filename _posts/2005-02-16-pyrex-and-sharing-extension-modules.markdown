---
layout: post
title: "Pyrex and sharing extension modules"
date: 2005-02-16 22:21
comments: true
categories: 
---

I'm using <a href="http://www.python.org/">Python</a> and C in one of the rather large projects I'm working on, and I'm using <a href="http://nz.cosc.canterbury.ac.nz/~greg/python/Pyrex/">Pyrex</a> to provide the bridging code.  Once I got over some of the tricks involved in sharing types between extension modules, it was cooking with gas.

Now I have a <a href="http://www.linux1394.org/">Firewire</a> camera running live in a <a href="http://www.wxpython.org/">wxPython</a> window being displayed using <a href="http://pyopengl.sourceforge.net/">PyOpenGL</a>.  Very cool indeed...  Now all I have to do is hack in YUV support into <a href="http://gandalf-library.sourceforge.net/">Gandalf</a>, and I'll be set!

Writing extension modules in Pyrex is dead easy, sharing them just takes a little extra know-how.  You have to declare the <tt>cdef</tt> functions along with the data members in your <tt>.pxd</tt> file.  Then the implementation goes into the <tt>.pyx</tt> module, which is compiled.  The trouble I came across was calling <tt>cdef</tt>ed functions between modules that used a common C type defined in another module.  Clear as mud?

The solution was to declare the C <tt>typedef</tt>s in their own <tt>.pxd</tt> and import those into both, thus putting all the C types in their own namespace.  Then two different extension modules can use the same C types from Pyrex code and all's well.

## 2013 Update

These days, you would likely use the [`ctypes` module](http://www.python.org/lib/ctypes/), which allows you to transparently
invoke C functions from Python code.
