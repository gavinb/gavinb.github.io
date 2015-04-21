---
layout: page
title: "libfg - video4linux framegrabber library"
date: 2003-01-13 11:23
comments: true
sharing: true
footer: true
---

 * The current version supports USB cameras, and features a sample app for
   live video display!

## Introduction

The **libfg** library provides a simple high-level C API for controlling
Video4Linux devices, including analog video cameras, TV and webcams.

## Overview

Linux has support for TV tuners and frame grabbers. This is provided by the Video4Linux API, which has been around since roughly kernel 2.2. V4L is actually an interface implemented by a number of different drivers or kernel modules that support the different types of hardware. Since it is a kernel level interface, it is driven entirely by ioctl() calls, and is not particularly friendly to use. It is also not particularly well documented.

The purpose of libfg is to provide a simple, high level interface for controlling TV tuners and frame grabbers. This insulates the developer from having to fiddle around with the low-level details. An example below shows how easy it is to use. It is hoped that this will enable developers to write more applications that exploit this hardware, and create some interesting programs.

## Features

The current version of libfg supports most of the major features provided by the Video4Linux API (Version 1). This includes:

 * Double-buffered capturing (depending on driver)
 * Setting a capture window (cropping)
 * Set brightness, contrast, etc (in percent)
 * Control TV tuner (in MHz)
 * Supports multiple norms (PAL, NTSC, Secam, etc)
 * Supports different frame formats (eg. RGB32, RGB24, YUV)
 * Can save frames to disk (pgm)

## Hardware Support

Any V4L device supported by the kernel will work with **libfg**. This includes
capture cards, TV tuners and webcams.  The most common type of hardware is
probably cards based on the BT848/BT878 chipset, although plenty of others will
also work. These days, TV tuner/capture cards are surprisingly affordable too.
Get yours today! For more details on particular cards, consult the documentation
that comes with your kernel.

We have tested libfg on the following hardware:

 * Picolo (BT878)
 * FlyVideo 98 / Chronos Video Shuttle II (BT878)
 * Logitech Webcam Express

**Note on Firewire/1394 devices:** it should be possible to use a Firewire camera with the loopback driver.

## Kernel Support

You may need to build your own kernel (Shock! Horror!) if your distro doesn't already come with the Video4Linux modules. Debian doesn't by default, while Mandrake does. You will need to enable at least Video4Linux, I2C (for the tuner), and the right module for your card.

## License

The libfg code has been released under the [Lesser GNU Public License
(LGPL)](http://www.gnu.org/copyleft/lgpl.html). This is to ensure that the code
can be used in as many applications as possible (even proprietary or closed-
source projects if you really must), while ensuring that improvements to the
library itself get sent back to the community. Please read the license in its
entirety and understand it before using the software - a copy is included in the
source in the file LGPL.

## Projects

Currently libfg is used in the following projects:

 * [The University of Melbourne RoboCup Initiative](http://www.cs.mu.oz.au/robocup)
 * [Player/Stage](http://playerstage.sourceforge.net/)
 * Mezzanine

Its main application thus far has been in frame-rate (~40ms) image processing
for robotics applications.

If you use libfg in your project, please let me know so I can add you to the
list.

## Download

For now, you can just download the source tarball (using the link below):

 * [libfg-0.3.1.tar.gz](/files/libfg-0.3.1.tar.gz)(Source tarball, 122kB)

## Documentation

API reference documentation is provided in the above tarball, in both HTML and
PDF formats (thanks to [Doxygen](http://www.doxygen.org/)).  The included
samples show how to use it; it is fairly straightforward. The device is always
initialised to sensible defaults, so after `fg_open()` returns, you can start
grabbing right away.

## Examples

There are two example progams included - test-capture, which exercises a number
of different calls in the library, and **camview**.  The **camview**
program uses [libsdl](http://www.libsdl.org/) to perform live
streaming of video images into a window.

## Building

Just running make will give you a static library (.a) to link with. For now,
that will have to do until I sort out dynamic linking of shared libraries and
versioning and all that it entails. The README has info on how to build the
Python bindings, which is pretty simple.

## Sample User Code

Here is an example of how easy it is to use libfg to control your frame grabber in C:

{% highlight C %}
#define TV_ABC 64.250f

void test()
{
  FRAMEGRABBER* fg = NULL;
  FRAME* fr = NULL;

  // Open the default device
  fg = fg_open( NULL );

  // Capture from our VCR

  fg_set_source( fg, FG_SOURCE_COMPOSITE );

  // Take a snap
  fr = fg_grab( fg );
  frame_save( fr, "vcr.pgm" );

  // Now get ABC TV
  fg_set_source( fg, FG_SOURCE_TV );
  fg_set_tuner( fg, TV_ABC );

  // Take a snap
  fg_grab_frame( fg, frame );
  frame_save( fr, "ABC.pgm" );

  frame_release( fr );
  fg_close( fg );
}
{% endhighlight %}

And here is an example of the preliminary Python support.  This script
was used to create the image at the top of this page.  (Gimp was used
to convert the file from PGM to JPG and scale it down a little.)

{% highlight C %}
#!/usr/bin/env python

import fg

g = fg.Grabber()

g.set_channel(182.250)

g.save_frame('sample.pgm')

del g
{% endhighlight %}

## Useful Links

Here are some links that may be useful:

 * [BTTV Mini-HOWTO (LinuxDoc.org)](http://www.tldp.org/HOWTO/mini/BTTV.html)
 * [Video 4 Linux HQ](http://bytesex.org/v4l/)
 * [Video 4 Linux Version 2](http://www.thedirks.org/v4l2/v4l2.htm)

## Archived Comments

### Comment:
AUTHOR: None
DATE: 08/11/2004 01:33:02 AM
python extension library
Well, your library seems to be very usefull however I don't succeed building the extension. Where can I found fgmodule.c?

Thanks

Guillaume

### Comment:
AUTHOR: Anonymous
DATE: 01/29/2005 10:48:10 AM
Grabbing a greyscale (8 bit) frame
Hi,

I'm starting to do basic machine vision for my robot project and I was very happy to find your library as most other ones are too big are complicated for running on a small embedded ARM linux robot.

I was wondering how I could capture a single 8-bit greyscale frame using libfg as I don't need colour at this point and would prefer to reduce the complexity of my CV code by using simple 8-bit pixel data.

Thanks for your great lib.

Stephane Gauthier
<a href="http://robotics.no-ip.org">http://robotics.no-ip.org</a>

### Comment:
AUTHOR: Anonymous
DATE: 02/16/2005 02:20:52 AM
Grabbing a greyscale frame
I found an easy solution.

If using the ov511 driver just load the module with option force_palette=1 for grey.

Or simply use VIDEO_PALETTE_GREY when initializign V4L.

Take care,

### Comment:
AUTHOR: Anonymous
DATE: 03/03/2005 06:52:44 AM
python extension library
I have the same problem. Â¿where is that file?

Tankyou.

Knolan.

### Comment:
AUTHOR: gavinb
DATE: 05/16/2005 11:10:43 PM
Finally - an update
The download attached to this article contains a refreshed copy of the source, including the missing fgmodule.c.

### Comment:
AUTHOR: Anonymous
DATE: 05/31/2005 02:32:23 AM
64-bit support
There are a few minor changes I'd suggest, so that it compiles properly under 64 bits:

<pre>
capture.c, line 164:
    if ( (int)fg->mb_map mb_map fbuffer.base );
change to:
    printf( "  fbuffer.base         = 0x%08lx\n", (long)fg->fbuffer.base );

catpure.c, line 648:
    printf( "  mb_map      = 0x%08x\n", (int)(fg->mb_map) );
change to:
    printf( "  mb_map      = 0x%08lx\n", (long)(fg->mb_map) );
</pre>

### Comment:
AUTHOR: David
DATE: 08/15/2005 10:42:34 PM
"Inappropriate ioctl for device" error returned
Hello,

I have tried the grabber library with my USB Logitech Quickcam Pro 4000 webcam but when I ran the sample camview, it returned the error "Inappropriate ioctl for device" followed by a segfaut. The driver for the cam is pwc and is working fine. I can still use GnomeMeeting to see video from the cam. Do you have any ideas on this problem.
Thanks you very much in advance.

Kind regards,
David

Comment:
AUTHOR: yaayaa
DATE: 03/31/2006 06:23:41 AM
Hello and congratulation for
Hello and congratulation for your library.
I use the python API but I don't find the grab_frame() method.
I would like to access the frame without using frame_save(), and then to process it with PIL.

Does this method exist (I could not find it using python introspection) ?
If not when do you plan to add this method to the python API ?
Thanks by advance.

yaayaa

### Comment:
AUTHOR: Ken Larson
DATE: 06/19/2007 12:50:12 AM
Missing munmap in fg_close
You can't open the same video device twice without this change:

    int res = munmap(fg->mb_map, fg->mbuf.size);
    if (res != 0)
        perror( "fg_close(): munmap failed" );

at the beginning of fg_close

These folks have discovered it too:

<a href="http://playerstage.cs.umn.edu/bzr/player.marsupial/server/drivers/camera/v4l/v4lcapture.c">http://playerstage.cs.umn.edu/bzr/player.marsupial/server/drivers/camera/v4l/v4lcapture.c</a>

### Comment:
AUTHOR: remaxim
DATE: 07/21/2007 09:54:09 PM
Hi,
Hi,
I've downloaded your libs and wanted to build the python building... 
Unfortunatly the only thing I get is this: 
$ python setup.py build
running build
running build_ext
building 'fg' extension
gcc -pthread -shared -Wl,-O1 build/temp.linux-i686-2.5/fgmodule.o -L. -lfg -o build/lib.linux-i686-2.5/fg.so
/usr/bin/ld: cannot find -lfg
collect2: ld gab 1 als Ende-Status zurÃ¼ck
error: command 'gcc' failed with exit status 1

Do you know how to solve this?
thanks
Maxim

### Comment:
AUTHOR: das6745
DATE: 09/19/2007 08:20:49 PM
SDL.h
Actually i have an error while making: 
gcc -g3 -Wall -I. -lm -o camview camview.c \
                        libfg.a -lSDL -lpthread
camview.c:17:21: error: SDL/SDL.h: No such file or directory
so what is SDL.h? i have libsdl already install, have its soutces but dunno what must to be an SDL.h =). So if somebody knows how to repare it pls help

### Comment:
AUTHOR: Anonymous
DATE: 10/16/2007 10:08:30 AM
Problem with libfg
Hi, I downloaded your library and was able to compile it,but have problems executing the samples. None of them run ok, camview nor test_capture. I got following error: 

fg_open(): mmap buffer: Invalid argument
Fallo de segmentación (core dumped)

I have tried it with two cameras, one connected to a capture card (Kworld TV Terminator: saa7134), and the other a USB camera using driver spca5xx. I know saa7134 is a driver for V4L2, but the USB camera uses a V4L driver, so could you please help me? I compile the library, copied the libfg.a file to /usr/lib, and ran ldconfig. What can the problem be? Im using Ubuntu 6.10...

Thanks in advance for your help....

### Comment:
AUTHOR: Adalbert Prokop
DATE: 01/19/2008 04:18:55 AM
Patch for RGB565/555 file formats
When I was exploring this beatiful small library I encountered some problems with saved frames which were grabbed in RGB565 mode. The file format was invalid. The saving routine for RGB565/555 is wrong. I've generated a patch file. After patching files grabbed in RGB565 are shown correctly.
I'd like to send you the patch, but how can I contact you? I could not find an e-mail here... I'll post a link here ASAP - right now my webspace is down.

BTW: I like your lib very much. It saved me a lot of time and sped up learning V4L. Thanx!

### Comment:
AUTHOR: Adalbert Prokop
DATE: 01/22/2008 02:00:22 AM
Patch for saving RGB565 and RGB555 format
Here is the patch I mentioned in my last post.
<a href="http://www-student.informatik.uni-bonn.de/~prokop/rgb565and555.patch">patch for rgb565 and rgb555</a>

COMMENT:
AUTHOR: echoline
DATE: 05/01/2008 05:47:00 PM
mmapfix.patch
Anonymous:  you probably never checked back here.  if anyone else has this problem just type patch in the libfg-0.3.1/ directory and paste this patch, pressing ctrl+d after pasting to signal EOF to patch program.

<pre>
--- capture.c.orig      2008-05-01 00:26:15.000000000 -0700
+++ capture.c   2008-05-01 00:31:52.000000000 -0700
@@ -532,6 +532,9 @@
 
 FRAME* fg_new_compatible_frame( FRAMEGRABBER* fg )
 {
+    if ( fg == NULL )
+        return NULL;
+
     return frame_new( fg-&lt;window.width,
                       fg-&lt;window.height,
                       fg-&lt;picture.palette );
</pre>

good job on the library, dudes who wrote it!

### Comment:
AUTHOR: echoline
DATE: 05/01/2008 05:57:33 PM
mmapfix.patch
also, make line 165 of capture.c look like this:
<pre>
    if ( fg->mb_map == MAP_FAILED )
</pre>

### Comment:
AUTHOR: Anonymous
EMAIL: The_Ironraver@web.de
DATE: 06/11/2008 10:21:07 PM
Where can I find this patch?
I've got the same problem, so where can I find your patch?
Thanx!

### Comment:
AUTHOR: admin
DATE: 06/16/2008 06:08:20 PM
Patch submission
Hello, please email your patch to gavinb at this domain.  If it is straightforward and builds cleanly, I will incorporate it into an update.

Please note that I no longer have any V4L hardware, and so I am unable to test the library. That is also why it has been so long since it has been updated (apart from being incredibly busy!).

Thanks - Gavin

### Comment:
AUTHOR: freespace
DATE: 07/15/2008 07:28:24 PM
I don't see how that helps.
I don't see how that helps. The error occurs because the mmap call fails. I am having the same problem, it and appears libfg is asking for 17039360 bytes and mmap is unhappy about it. I am still trying to get it running :/

### Comment:
AUTHOR: freespace
DATE: 07/15/2008 10:16:04 PM
Patch and unoffical repository for libfg
I have a <a href="http://git.pictorii.com/?p=libfg-unoffical.git;a=blob_plain;f=libfg-0.3.1-unoffical.patch;hb=17ccbf6fb0f5e04dc8e87353eef8d9d375ac1057">patch for libfg 0.3.1</a> which allows it to compile and run under kunbuntu hardy using bttv drivers, without the mmap "invalid argument" error.

The patch integrates the work of Adalbert Prokop and other fixes along with my own (excluding however the 64bit fixes since I can't test them myself). I have setup a "maintenance" repository to handle integrating these and possibly future patches. It is my hope gavinb will have some free time in the future, and make my efforts redundant.

Many thanks for gavinb for sharing libfg with us all. Now that I have it working, time to get coding :-)

Cheers,
Steve

### Comment:
AUTHOR: freespace
DATE: 07/16/2008 01:13:31 AM
Swig generated python interface
Since gavinb is busy, I have taken the liberty of creating a very un-pythonic interface to libfg using swig. You can find the required files in my <a href="http://git.pictorii.com/?p=libfg-unoffical.git;a=summary">unofficial libfg git repository</a>. Specifically you need the .patch, the .i and the Makefile. test_capture.py can be used to test the python module once it is built - it is a straight port of the C code on this page.

Cheers,
Steve

### Comment:
AUTHOR: woodstock
DATE: 09/13/2008 08:04:23 PM
Hi, I came across the same
Hi, I came across the same error and found the following solution.
Download and install libsdl1.2-dev.
I'm running Ubuntu and got the library package with the according headers through Administration -> synaptic package manager.
