---
layout: post
title: "Optimize this..."
date: 2005-04-13 10:54
comments: true
categories: 
---

I'm working on an industrial image processing system, and it's coming together pretty well.  I hadn't tried optimising anything up until now, as I wanted to get all the features implemented and do some profiling.  "Premature optimisation is the root of all evil", as they say...

But as of the last few days, the system has reached the point where most of the key features are working on the imaging unit, certainly enough for meaningful performance tests.  And since it is a near real-time system, performance has always been a key concern.  A good excuse to start some hunting for some optimising opportunities...

## Classifier

The target for a full processing cycle is 100-120ms, although I'm aiming for more like 90ms.  Without any optimising whatsoever, the processing cycle was taking far too long at something over 200ms.  One stage, the classifier, was taking just over 30ms per frame, which seemed excessive considering its primary role.  I started looking at the main loop, and it became clear that there was a lot of extra code calculating stuff only used during calibration.  Making that optional saved 5ms per frame.

Looking at the guts of the inner loop, there staring at me was the next obvious candidate: a function call, used to retrieve the YUV pixel (or YCbCr CCIR-601 if you prefer) components from the YUV:422 packed frame.  I rewrote the pixel access as a macro, and got it down to 15ms.  Nice, but I'm sure can do more.

At this point I realise that I haven't even tried the obvious thing of adding '-O3' to the compiler flags.  I rectify this oversight, and this gets us down to 10ms.

I restructure the loop some more, and remove even the test for the calibration code from the inner loop, so all it is doing is the lookup table.  I also remove a histogram calculation that I didn't end up needing to refer to, and now get it down to around 4ms.

I was reasonably happy at this point, as the code was still perfectly readable and modular, and I didn't have to resort to assembler, obfuscation or kinky tricks.  There's one more thing I can do that might shave off a few cycles, but I had to move on to the next target.

## Capture

We are using Firewire cameras, specifically the [Basler 601fc](http://www.basler.com/).  The state of [IEEE 1394](http://www.linux1394.org/) support under Linux is somewhat in flux at the moment, and I wrote a layer to abstract away the details of Firewire camera control.  This not only simplified the layers above, it made us insensitive to which particular underlying library we would ultimately use (currently libdc1394).  It also made it nice and easy to write a Python wrapper (using Pyrex of course).

When I first wrote this fwcam layer, I used the simpler of two capture models, and once it was working, left it at that.  But when I added a high resolution timer to profile the image capture code, I discovered that just the capture was taking over 40ms.  This seeemed excessive too, so I went back to re-read the docs and code and see how I could speed things up.  It turns out that there was a DMA version of the capture function, and although it required some extra buffering, it wasn't much effort to change the fwcam layer to take advantage of it.

I was rather shocked to see, once I had rebuilt the library, the capture time was down to under 2ms!  I was expecting a big difference, but even this much was a surprise.  Another nice feature is a non-blocking variant of the DMA capture call that could return immediately if no frame was available.  This would prove useful in the async triggering protocol.

## But wait - there's more...

There are many more opportunities, I'm sure.  Through some careful analysis, strategic placement of some high-resolution (microsecond) timers, and restructing and refactoring of the code, I managed to shave around 26ms per frame in the first case, or about 8 times faster.  But the most important thing is that the code is still readable and maintainable, not some mess of macros and funky shortcuts nobody else but me can grok.

Of course, I have been looking at the MMX extensions in GCC recently, so... :)
