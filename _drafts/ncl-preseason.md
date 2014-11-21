---
layout: post
title: NCL 2014 Writeup - Pre-Season
description: null
category: null
tags: []
published: true
excerpt: "Updating the BIOS, installing software, configuring things..."
---

{% include JB/setup %}

## About This Series
(MAKE THIS A BETTER INTRO)
I participated in the 2014 Fall season of the National Cyber League. During the Pre-Season (basically a qualifier), I decided that I should post a write-up on how I approached each set of problems.

## The Low-Hanging Fruit
The first two sets of flags were Windows and Linux password hashes. For those who are unaware, John the Ripper (ADD A LINK) will make short work of these. (JOHN JUMBO)

## Network Captures

## Recon

Now we're getting somewhere. Two hosts were in the "Recon" section. Recon 1's flags were essentially "Starting from the first open port, what is the [nth] open [TCP/UDP] port on the system?"
Easy peasy, fire off an nmap scan with UDP at the machine, and fill in the blanks.
(MAYBE ADD THE COMMAND YO)
Recon 2 was a slightly different beast. The first flag asked on what port an HTTP/1.1 compliant service was running, and the rest were simply "What is the [1-6]th flag on this target?" An nmap scan revealed a non-standard port running an HTTP server, and upon visiting the site at that port, a simple HTML page was delivered. I did not save a copy of this page, but it was similar to the following:
(HTML FORMATTING)
flag 1 is big
flag 2 is title
flag 3 is white
flag 4 is tiny
flag 5 is commented and base64
(/HTML FORMATTING)
Many people in the IRC support chat were utterly confused as to where to find flag 6. It was located at [ip address]:[port]/flag.

## OSINT
All of the OSINT challenges in the preseason (and indeed in the regular season as well) were repeat challenges from the 2012 game, the answers to which could be found in a PDF published by the NCL (LINK)

## Web

## Log Analysis

## Crypto

Crypto 1 was simply a base-64 encoded string. Crypto 2 was rot-47. 
Crypto 3 was an encrypted zip file alongside a jpg. (EXPOSITION)
(POST CRYPTO 4)
Crypto 4 was multi-layered, and I was only able to get past the first two levels. 4.1 was an atbash encoded text blob. When decoded, we're presented with the first flag and another encoded string. This string was base-64 encoded morse code, represented with periods and hyphens. On decoding this string, we are presented with the second flag and another encrypted string. That's as far as I was able to get.

## Enumeration

Enumeration 1 presented a website with a number of pages protected by .htaccess files, as well as a 'change password' page with no such authentication. Not having any information to feed into the change password page, I fired up nmap to see what else was open on the target (this is enumeration, after all). The only other open port was 79, which is finger. And it was, indeed, finger, which I verified by asking about a user that I knew for a fact would exist- root. Brute-forcing over a network is never a great thing to do, so I spent some time trying to run SQL injections on the change password page and a few finger exploits that worked on old Solaris systems, all to no avail. With other options significantly discounted, I brute-forced finger. I downloaded a username wordlist, and fed it to the following shell script:
(INSERT FINGER SCRIPT)
When feeding the wordlist to my script, I piped it through grep so that it would only bother asking for usernames beginning with z (after all, I didn't want to hammer at the target network too hard).
(RESULTS)

## Wireless

## Exploit

Exploit 3, at first glance, was an empty webserver. When in doubt, nmap. Nmap revealed port 1234 to be open, so naturally let's hit it with a web browser. The result of this was... disappointing. It took me a while to figure out, but the tl;dr version of this particular process was that eventually, the web browser would occasionally dump out a binary file. So, I saved the binary and started digging in to it.
The process for Exploit 4 was quite similar, with only slight variations at the very beginning and the very end. The nmap scan again revealed that 1234 was open, but there was nothing hosted on that port. A more intense nmap scan revealed that 1235 was also open, and 1235 on Exploit 4 behaved the same way as 1234 on Exploit 3. This does also follow the order in which I approaced these problems- I had the feeling that they would be similar, and so did both this and all of the following steps in tandem against both targets.
So now we've got two binaries, let's see what they can tell us.
(INSERT FILE AND STRINGS)
So, we know that they're both 64-bit executables, and Exploit 4 is (as should be expected) a bit more complicated.
