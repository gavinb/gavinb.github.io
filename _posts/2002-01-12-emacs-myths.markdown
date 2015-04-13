---
layout: post
title: "Emacs Myths"
date: 2002-01-12 02:46
comments: true
categories: 
---

A recent <a href="http://www.newsforge.com/">article</a> on NewsForge brought up
one of the oldest rwars on the planet (almost as old as the operating system
rwars), asking which editor is best: vi or Emacs?  I was inspired to write my
own thoughts, findings and opinions after reading this article.  Especially
since most of the arguments I read were trolls sprouting opinions and
regurgitating fallacies, rather than any informed sort of debate.  So here I am,
with my own 2 cents.

<!--more-->

## History

First, a few words on my history with editors.  I am an Emacs user, and have
been for around 4 years.  Over the years, I have used a wide variety of editors
on a number of platforms platforms.  Some of my favourites were Brief (small,
fast, powerful), E/PM (powerful, intuitive) and Visual SlickEdit (very much like
Emacs, but with crap default keybindings).  And yes, for a number of years,
whenever I used Unix I used vi.  I found the modal editing system to be arcane,
the keybindings obscure, and the functionality limited.  But I still used it for
a long time,  and knew it reasonably well.

One day I was reading the newsgroups, when a discussion (which quickly descended
into a flamewar) on the relative merits of vi and Emacs broke out. Based on what
people were saying, I thought this thing called Emacs sounded interesting, and I
decided to check it out.  It was  installed on one of the Unix machines at Uni,
so I tried it out and went through the tutorial.  I found the keys a bit weird,
and felt a bit lost.  Unfortunately, this was around the time I was working on a
major project, and it was pretty poor timing to consider changing editors.  I
forgot about it, until around 6 months later.  I was working on a project over
summer, and I thought it was time to try this new editor again.  I was intrigued
by what I had read, as it sounded very powerful.  So I fired up the tutorial,
and learned the very basic editing keys.  Then I started to actually use it
while working on a compiler I was writing.  I found that once I knew the basics,
I could get by.  And once I discovered how to take advantage of the help system,
I could just look things up whenever I needed to do something but didn't know
the keys. After a while, I started seeing the patterns in things.  There is,
believe it or not, a system to the key mappings, which starts to make sense once
you know enough of them.  But by the time I had moved past the 'novice' user
stage, I noticed three striking things.

First, I was rapidly discovering that Emacs was much more than an editor. It was
an incredibly powerful text processing system with a Lisp interpreter at its
core, which could be extended in any way imaginable.  Second, I found that I
could learn things incrementally.  Whenever I needed to do something new, such
as run a spell check on a document, I would just look up the help system and
there it was.  Which leads me to the third thing - after you use Emacs for a
while, you realise that every time you need to do something, somebody has
thought of it already, and it's there.  You can tell just by using it that it
was designed by programmers for programmers.  So there are some of the reasons
why I now call myself an Emacs user.  I've just about tried them all, and I've
chosen the one I believe is the best, for many reasons.

## Fact or opinion?

I went looking for pages advocating vi over Emacs, and found very little
substance.  Most of the stuff around seems to be opinionated trolls spouting the
usual "Emacs sux" lines, with no substantive arguments to back it up.  I have
actually been the target of active ridicule by fellow  programmers who use vi.
The interesting thing was that they all said the same two things: Emacs is big,
and Emacs is slow.  But beyond that, they actually knew very little about what
Emacs could do.  So let's dispell a few myths.

## Emacs is big

Yes, it is - the latest version of Emacs (v21) takes up around 40Mb on my
machine.  For comparison, the latest version of Vim takes up around 12Mb. But
really, so what?  There is an enormous amount of functionality in the full
package - you really do get a lot of bang-for-buck. Besides, these days, hard
drives are so cheap that the difference here is meaningless - a few cents.  If
you are really concerned about size, it is pretty easy to make a minimalist
distribution of Emacs of only a few Meg.  And there are alternatives such as
microEmacs which may suit too (this is is what Linus Torvalds, creator of Linux
uses).

## Emacs is slow

Slower to load perhaps, but certainly not slow to run.  Compared to vi, an Emacs
system configured with a bunch of packages usually takes longer to load; this is
true.  But once loaded, Emacs is extremely fast with just about every operation.
And a fundamental distinction must be made here - unlike most vi users, Emacs
users tend to open a session at the start of the day and leave it running,
opening and closing multiple buffers.  This is a very different work style to
the typical vi user, who will open and close vi many times.  In this case,
startup time is obviously important, but the extra second or two for Emacs is
irrelevant.  For text processing, that is where the speed counts, and even for
multi-megabyte files Emacs is very fast.

## Emacs is bloated

Many people think that Emacs is bloated, in that it has built in web browsing,
mail reading, kitchen sink, news reading, etc. etc. Some people see this as an
advantage, some think it sucks. Much of it comes down to personal perference,
but don't forget you only use what you need. The extra functionality is there if
you want it.  You can always use a minimalist installation with just what you
need. People don't complain that Debian is bloated because it has over 4000
packages, because people can just install the packages they want.

## vim can do anything Emacs can

Crap. There are a zillion things Emacs can do that vi can't. The vast majority
of vim users wouldn't have a clue just what functionality is available in Emacs,
and anyone still wanting to argue this point should go reread point #3 above.

## Modal editors are cool 

Ok, I used to use vi. But the modality really bugged me. Now if you ask any
knowledgable person in HCI, they will tell you that modal apps are bad design.
They should know. Now I happen to prefer Emacs because it is not modal. But if
you seriously prefer vim because it is modal, or you like the keystrokes, then
fine - that is your opinion. It's about the only decent reason I've heard for
anyone using it.  So if you like that, great - enjoy it, I say. I won't argue
with your preference.  But don't start preaching about how it is somehow
inherently better - plenty of experts will disagree, and rightly so IMHO.

"People don't know that vi was written for a world that doesn't exist anymore."

Who said that? Bill Joy, creator of vi. You have to remember that vi was
designed for low-speed character terminals. But the world has moved on, as Bill
notes. Obviously vi is stuck in the dark ages - vim might be an improvement on
vi, but the fundamental design of modal editing no longer has a rationale.

## Empirical Comparisons

Instead of making wild pronouncements, I decided to do some empirical testing.
So I fired up both the console and the GUI versions of Emacs and Vim.  Here are
the results (according to 'top'):

    
    
      Program
      Size
    
    
      gvim
      emacs
      vim
      emacs
    
    
      8560K
      10M   
      8152K 
      8544K 
    
    

So yes, Emacs is bigger, but not by very much.  And I would argue that for that
size, you get a lot more functionality.  So for the sake of a few hundred
kilobytes, I don't think those who argue that Emacs is huge compared to vi have
much ground to stand on in light of these figures.

Now comes the speed tests.  We want to test startup time only, so I ran the same
test on each editor 5 times, then averaged the results.  I had already loaded
both prior to the tests to prime the cache. Each editor was passed a command
line which would make it quit straight away.  This was about the best comparison
for startup time I could come up with.  If anyone has a better idea, I will
listen.

      Program
      Emacs
      Vim
    
    
      Size
      0.348s
      0.428s
    
    
      Command Line
      emacs -q -nw --execute (kill-emacs)
      vim -c q

Now this was a big surprise - Emacs actually came out faster in startup time!  I
was certainly not expecting this.  I know that it can slow down if you preload
lots of packages in your customisation file (which is why you should use
autoload) but I wasn't expecting this.

## Conclusion

Well, what can we say?  Emacs is much bigger in terms of disk space, but it has
lots more functionality out of the box.  It is a little bigger in terms of
memory footprint, with no extras loaded.  And it can even load as quickly as vim
if you want it to.

Now I am not trying to convert vi users to Emacs, so don't get me wrong.  I just
wanted to set the record straight about those ignorant vi users who deride Emacs
out of an ill-deserved sense of superiority.  Now if you prefer vi, then great -
use it! I'm happy for you!  But don't turn around and try to argue that it is
somehow intrinsically better than Emacs unless you have some facts and cogent
arguments to back up your words.
