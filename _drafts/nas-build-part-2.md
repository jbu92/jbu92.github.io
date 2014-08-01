---
layout: post
title: "Nas Build - Part 2"
description: "Software"
category: 
tags: []
---
{% include JB/setup %}

**ALSO GONNA WANT TO DO PRE-INSTALL**
## Software
I've yet to actually install the system as of the writing of this post, but I do know what I am going to do, so whatever, here it is.
I was initially going to go with [FreeNAS](http://www.freenas.org/) because I wanted something that was pretty much point-and-click, but after some evaluation via virtual machines, I decided I didn't like it. 
I looked at a few other push-button solutions, including [Amahi](https://www.amahi.org/) and [Turnkey Linux](http://www.turnkeylinux.org/) but decided that for the sake of my sanity (and, y'know, features) that I may as well just use an Ubuntu install and build it up from there.
So, first things first, after the inital system installation (having selected ssh server and samba server during the install), let's get everything else I want installed:
```
sudo add-apt-repository ppa:tuxpoldo/btsync
sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get install btsync stunnel -y
wget http://download-new.utorrent.com/endpoint/utserver/os/linux-x64-debian-7-0/track/beta/ && mv index.html utserver.tgz
tar xvf utserver.tgz
mkdir ~/bin
wget -O ~/bin/dropbox.py "https://www.dropbox.com/download?dl=packages/dropbox.py"
chmod +x ~/bin/dropbox.py
```
If you're not too Linux-savvy, here's the breakdown there:
1. First we add a repository that contains Bittorrent Sync
2. Then we update the packages that the system knows about, install all the available updates, and install btsync and stunnel
3. Then we grab uTorrent server and dropbox and throw them in ~/bin

Now, uTorrent server doesn't have its own init script, so I'm using the one from **HEY REMEMBER TO GO FIND THIS**.

With that, we can simply run `sudo service utorrent {start|stop|restart}` whenever we like.

Now, uTorrent server does not currently support SSL, so I'm using a utility called stunnel to open up an HTTPS port that will forward to the local port that uTorrent is listening on.
**CONFIG GOES HERE**

