---
layout: post
title: "Installing OpenBSD"
date: 2003-01-12 04:25
comments: true
categories: 
published: false
---

So I decide to install OpenBSD 3.2, just to have a play.  It starts off nice and
simple...

Then it comes to fdisk.  Now I've installed lots of different operating systems,
and I've chopped up disks using lots of different tools.  What I found odd was
the navigation in the fdisk tool.  The usability factor is about minus a
thousand; exposing the lowest possible level of partitioning is not necessarily
a good match for someone who "just wants a 650Mb primary partition".

I had a number of primary and extended partitions on the big disk where I was
installing it.  When it first came up, it printed 3 tables of info.  I realised
this must be first the primary partitions, then the other two tables showed the
contents of the extended partitions.  Fine... have a quick look at the help to
get familiar... Have a read of the manual... Ok, so lets try the select command
- that's gotta be safe.  Type: select 1.  Not an extended partition... Ok.
select 2.  Cool - get some more guff on MBRs and so on.  Do a print, see the
contents.  Fine, now I want to see the top level.  select 0.  Nope.  It took
over 20 minutes of faffing about before I decided to try 'quit', only to
discover that this was the way to go "up a level" in the navigation, to get back
to looking at the primary partitions.  Nothing in the online install guide or
the manual or the FAQ were helpful in discovering this.  Best I send off a patch
for the docs...

Anyway, I changed an old 800Mb FAT partition to type A6 as a new home for
OpenBSD.  I saved the changes, and continued.

Next comes the slicing and dicing.  Well, BSD sure has some different ways about
it.  But it's always interesting to find new stuff.  But it's a bit intimidating
when you do a 'print', and discover that for some reason your entire hard drive
is listed as 'partition c' and you are politely reminded not to screw with it.
What's the deal with that??

Now thus far, I have a single 800Mb partition ready for BSD.  It's already
created, but both the online install guide and the FAQ show the first thing to
do is delete the partition.  Presumably this is not really deleting the
partition so much as deleting a single slice that is assumed to be taking up the
whole partition?

Just to keep things really simple, I decide to have a single root and some swap.
I will reinstall later once (a) I have some more space to play with, and (b) I
have figured out how to properly allocate slices and so on.

So I get brave and do a 'd a' to delete the 'a' partition. Then I do an 'a a' to
add a root partition, and 'a b' to add a small swap partition.  During this
process, I can't see where it is you are shown the available or unallocated
space (which would be rather useful at this point).  You really have to know
what you are doing, and I imagine planning this on paper would really help.

Next the formatting, which goes fine.  I choose another hostname (taken from
Blake's 7) and this one is now 'orac'. Choose a few more little things, and then
come the packages.  I select all but X (the default) and away it goes.

The final stage goes by without a hitch, then I'm prompted to reboot.  I reboot
and up it comes. Although I notice that, without my go-ahead, it has nuked the
MBR boot loader that I had installed, so now I will have to backtrack to boot
any of the other systems.

Anyway, just when I think I have a working installation, it gets nearly all the
way through the bootup sequence, gets to "setting tty flags" and then just
hangs.

A bit of googling turns up some leads, and I discover that this was a known
problem with the pccom drivers at around v2.3.  Since I am running 3.2 I'm
surprised it is still a problem :-(.

Ok, now this is really weird - I spent several hours outside working in the
garden.  I come back and it is still hung.  I hit return a few times, and
nothing.  Then I turn back to the other machine where I am typing this, do the
search, then when I turn back to reset the new BSD install, it is sitting there
with a login prompt.  Weird... It mostly works, but it feels like the keyboard
support is still a bit dodgy.

I have installed it on an oldish Pentium box, with an AMI BIOS.  Nothing unusual
about it; very plain hardware, no exotic cards or anything.  Ah well, I guess
now begins the task of actually using the thing.

I guess, my comments on the install process would be: eck.  The whole
partitioning thing was far more painful than it needs to be.  There is just a
huge mismatch between the task that the user is wanting to perform (set up
partitions of certain sizes) and the interface (which exposes the physical
details at the lowest level).  Apart from that, it was basic but did the job.

Now on to using it...
