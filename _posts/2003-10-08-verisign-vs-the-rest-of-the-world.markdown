---
layout: post
title: "VeriSign vs The Rest of the World"
date: 2003-10-08 21:25
comments: true
categories: 
---

On the 15th of September, the top-level Domain Name Service (DNS) for `.net` and `.com`, fundamental to the entire Internet, was hijacked - by the very custodian entrusted with its upkeep.
<!--more-->
Some time ago, VeriSign (better known for their rather pricey digital certificates) purchased Network Solutions, and now acts not only as a domain name registrar, but operates some of the top-level DNS services worldwide.  They have done a fairly good job up until now... when they decided to cash in on their DNS operation, and redirect typos to an ad-driven web site.  And effectively undermine part of the fabric of the net in the process...

## Background - So what is DNS?

The Internet is built upon the TCP/IP protocol suite. Every machine connected to the net has a number, called an IP address, and machines use this address to communicate.  But since numbers are not very meaningful to us humans, there is a way to give all these numbers a name.  And that's where Domain Name Service (DNS) comes in.  Whenever you type `www.theage.com.au` into your browser, the first thing your browser does is look up the IP address corresponding to this name.  DNS works in a hierarchy, so first it checks `.au`, then `.com.au`, then `.theage.com.au` and finally `www.theage.com.au` before it gets the address.  And each level there is a different server that knows about that level of the hierarchy.

It is extremely important to note here that web traffic is by no means the only internet service to use DNS.  On the contrary, virtually *all* internet protocols use DNS at some point.  Email, newsgroups, time servers, remote access shells, streaming media, anti-spam software - virtually all software that uses the net uses DNS.

## So what did VeriSign do?

They added what is called a "wildcard" to the top-level DNS that handles anything ending with .com or .net.  So if someone tries to look up a bogus domain name, it will give back the address of their webserver called SiteFinder.  And their web server (assuming the error is coming from a user running a web browser) gives you a search screen - plus a whole lotta advertising.

## Why is this bad?

It used to always be that if you tried to refer to a domain that didn't exist, you would receive the `NXDOMAIN` (non-existent domain) error.  Your software, whether it be a web browser, email client, or anti-spam software, would receive this error and handle the situation appropriately.  And obviously the appropriate response to an invalid domain name is different depending on the service.

For example, to filter out spam using forged email addresses, it is standard procedure to for the email software to first look up the sender address and make sure it is at least coming from a valid domain.  If we get an `NXDOMAIN` response from the DNS server, we immediately know something is fishy.

But now, irrespective of the software or protocol, any domain lookup that would have ordinarily failed, returns `sitefinder.verisign.com`.  So if you typed in `www.no8way8this8is8a8real8domain.com` you would be sent straight to SiteFinder, where they will hit you with advertising, and maybe even try to sell you a domain.  And now your spam filtering software can't tell if the domain was really forged or not.

The implications of this are horrendous - name lookups will never fail.  Even completely bogus names.  Forgeet the web surfers that this is designed to target - this affects every protocol, because DNS is used by just about every protocol.

## What does it mean?

This is a blatant abuse of responsibility.  There is clearly a conflict of interest when VeriSign are abusing their control of the root name servers to drive unsuspecting users to their site for advertising revenue.  It is a violation of the established protocols for DNS resolution.

After serious threats from ICANN, VeriSign recently suspended the wildcard redirection to SiteFinder.  They argue that they are providing a "service" to users; they say they should be allowed to "innovate".

The problem they are ostensibly trying to solve is to help wayward web surfers when they mistype a URL.  Simple as that.  But rather than solve the problem on the client, and let the web browser decide how to handle the error condition and provide some sort of (customisable) searching facility (I believe AOL and MSN already do this), VeriSign have solved the problem on the server, in such a way that it affects not just web browsers but virtually everything else.

If it weren't for some very clever sysadmins and BIND developers, the impact would have been much worse than it was.  But already we have seen how a company driven by greed can take half the net hostage.  Let us hope that ICANN and the regulatory powers see to it that fundamental services such as DNS remain unpolluted, and do not get hijacked for commercial gain.
