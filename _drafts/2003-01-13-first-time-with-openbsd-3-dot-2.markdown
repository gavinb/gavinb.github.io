---
layout: post
title: "First time with OpenBSD 3.2"
date: 2003-01-13 10:08
comments: true
categories: 
published: false
---

So I have now installed OpenBSD 3.2.  I had a problem post-install, something to
do with the tty settings and a pccom driver.  This still isn't resolved; it just
"came good" after a little while.  So...

So all I have is a root user, and I log in as that.  Then it admonishes me for
logging in as root, and tells me I should 'su' instead.  Fair enough - it is
supposed to be rather anally-retentive security-wise after all.  Anyway, so I
decide to add a user.  Not knowing how this is done under BSD, I try typing
adduser, and get a little usage printout.  Right, well lets just type 'adduser
fred' and get myself a new regular user.  It then warns me that it hasn't
created a new home directory.  Sheesh!  Why wouldn't I want it to do that?!
Surely a reasonable default would be to create a new home dir, set a default
shell, then copy over some sort of `/etc/skeleton` type files to get a
reasonable default starting install.  Now I have to manually do all that crap;
and probably miss something in the process.  It also didn't even prompt for
missing information (such as comment, default shell, or anything useful like
that).

Meanwhile, the keyboard is still doing strange things every once in a while,
like inserting extra characters.  And the default shell obviously isn't bash, as
none of the keyboard shortcuts I'm used to work here either. Anything other than
alphanumeric keys are greeted with '^[A1' style characters; it's a bit raw.  I
was going to complain about the lack of virtual terminals, but I just found out
(by accident, reading the FAQ of all things) that it is C-A-F1, so I'm happy
with that!

Now I've just realised that my network card isn't working - it hasn't found the
right IRQ/IObase - it's an older ISA NE2000 clone card, so I will just have to
set it up manually.  Now where did I write those settings down again...?

(I must say, the documentation is as I had been told) very nicely done with
(OpenBSD.)

Er... time for a break (and some chores).  The saga will continue...

## Archived Comments

### Some booting and grub ideas

Date: 01/14/2003 10:54:17 PM

Well, you haven't actually written up the booting saga yet, it seems, but here
is a preemptive strike: [Booting many operating systems on one box](http://beta.mojain.com/41/node.php?id=8)

This is (as the url would suggest) on my beta drupal site--be kind.

### Grubby lilos...

Date: 01/17/2003 10:46:41 PM

I did actually end up writing up most of the booting saga... I basically used
Tom's RootBoot rescue floppy and chroot'ed to my old linux partition, reran lilo
and poof!  It works.

The rest of the saga - well, another blog reveals what happened next.

### User add user Add user add User...

Date: 01/17/2003 10:50:49 PM

Well I got bitten by the good old "adduser vs useradd" trick - again!

Of course, as everybody knows, there are two different ways of adding users, one
easy and one a PITA.  I stumbled across the latter while wanting and needing the
former.

Running 'adduser' by itself will do all the friendly prompting, setting up of
initial defaults and that sort of thing.  I must check back and see what the
docs say.
