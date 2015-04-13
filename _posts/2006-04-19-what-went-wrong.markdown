---
layout: post
title: "What went wrong?"
date: 2006-04-19 17:34
comments: true
categories: 
---

Writing *good* diagnostic error messages is hard.  But it's worth the effort.  And there's also some side benefits if you take the time, and it's not just avoiding hate-mail from your users.
<!--more-->

How many times have you run some software, only to have it fail with a message:

    Unknown Error: -3
    
    [Abort] [Retry]

There should be a third button: "Punch the developer who couldn't be bothered to put in a decent error message."

But it's actually hard to write good error handling, and that's why so many people don't.  Either that, or they are coding demigods who are so confident in their code they know it will never fail.  But a quick browse of the [UI Hall of Shame](http://www.userinterfacehallofshame.com) is enough to prove the point that even proprietary commercial developers goof up.

I'm just now writing a tool to import data into our software from CSV files.  And the variety of flavours of CSV, combined with different field labels, column ordering, field aliases, is staggering.  Add to that some fields are required, some optional, some should be ignored, then there are user-defined columns that come after the required ones we know about.  So a simple task as "import from CSV" turns out to be pretty complicated, and giving the user meaningful feedback to guide them to the problem is particularly tricky.

So to make things easier for the user, I've tried to be really friendly and provide comprehensive diagnostics.  This means checking every possible way things could go wrong, and ensuring that you have sufficient context in your code at each point to be able to provide *meaningful* errors.

So - which error message would you prefer to see when importing your data?

    Error: invalid field DS5301

or:

    Unexpected field 'DS5301' in header line 1 at column 11.
    Looking for required fields, still haven't found: id, family.

It takes time to craft an error message that reads like an actual English sentence, and conveys not only what went wrong but where and why, and what the software was expecting to find instead.  Error codes might be useful for the developer, but it's useless for the user.

So the bonus you get if you do write decent error messages?  It guides you to write your test cases, of course!  So after I added the above check to ensure all the required fields had been read before the user-defined fields, I wrote a data file that would trigger that error.  Now I have regression testing, I can make sure that the error gets picked up.  All this work for those ungrateful users...
