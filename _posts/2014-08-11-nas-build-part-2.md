---
layout: post
title: "Nas Build - Part 2: Software"
description: Software
category: null
tags: []
published: true
excerpt: "Updating the BIOS, installing software, configuring things...
---

{% include JB/setup %}

## BIOS update

So, first things first, I wanted to get the latest BIOS running on this new machine, as I'd heard that there were issues running Linux under the F3 bios. The process honestly could not have been much more complicated. Basically, there were two options- a bootable DOS flash drive (heh, NO.) or install Windows and use Gigabyte's software to get this thing updated. So, after installing Windows, I had to grab Gigabyte's App Center, and then the @BIOS utility. Initiating the BIOS install was simple enough from that point, but then I clicked around in the application while it was doing its thing (i.e. flashing the bios, with a nice little progress bar) and I couldn't get back to the page with the progress bar.

So, that was kind of a pain of a process, and was complicated even further by other issues I had with the network at the time, but I can't really blame the hardware for that.

## Software Selection

So now the question is what OS to use.

I was initially going to use [FreeNAS](http://www.freenas.org/) because I wanted something that was pretty much point-and-click, but after some evaluation via virtual machines, I decided I didn't like it. I looked at a few other push-button solutions, including [Amahi](https://www.amahi.org/) and [Turnkey Linux](http://www.turnkeylinux.org/) but decided that for the sake of my sanity (and, y'know, features) that I may as well just use an Ubuntu install and build it up from there.

## Data Transfer

Since I was moving from a Windows machine to a Linux machine, I was going to have to reformat my harddisks. Not wanting to lose data, I borrowed a spare 3TB disk from a friend and installed a working Ubuntu system on it, and installed that disk along with each of my disks (one at a time) in my new machine. The basic process was to mount the old disk, copy the old files on to the temporary disk, reformat the old disk, and copy the files back. I did the data transfer with rsync in a screen session, given the lack of monitor to keep an eye on things. Basically I just kept an eye on the hard drive activity light to know when the transfer was finished.

The only hiccup with this solution was that I had previously had to use a special tool to format my 3TB disk for Windows, so it was recognized as a 2TB disk and a ~750GB disk, and Ubuntu didn't recognize the 750GB partition, so I had to swap the disk back in to my Windows system to move the contents of the 750GB partition to the 2TB partition. Also, before restoring my files to the 3TB disk, I installed my OS, with the following partition map:

| EFI boot | System | Data  |
|----------|--------|-------|
| 50MB     | 20GB   | 2.7TB |

Once all of my data was copied over, and I had both disks installed in the machine, I was left with the following drive configuration:

| Partition | Format | Mount Point | Size |
|-----------|--------|-------------|------|
|reserved  | EFI Boot| not mounted| 50MB|
|/dev/sda1  | ext4| / | 20GB |
|/dev/sda2 | ext4 | /mount/disk1 | /media/disk1 | 2.7TB |
|/dev/sdb1 | ext4 | /mount/disk2 | 2TB |


## System Setup

This is probably the only part of this whole thing that other people might find useful.

So, first things first, let's install some software. SSH server and samba server were selected upon installation, so that leaves uTorrent, stunnel, BTSync, and dropbox:

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

### uTorrent
Now, uTorrent server doesn't have its own init script, so I'm using the one from [here](https://github.com/vortex-5/utorrent_initd/blob/master/utorrent). Naturally, the first few lines need to be modified to match my setup, so they now look like this:

```
UTORRENT_PATH=/home/jimbo/bin/utorrent/utorrent-server-v3_0 #where you extracted your utserver executable  
LOGFILE=/home/jimbo/bin/utorrent/log/utorrent.log #must be a writable directory  
USER=jimbo #any user account you can create the utorrent user if you like  
GROUP=users  
NICE=15  
SCRIPTNAME=/etc/init.d/utorrent #must match this file name  
```

With that, we can simply run `sudo service utorrent {start|stop|restart}` whenever we like, and more importantly, can tell it to start on boot with `sudo update-rc.d utorrent defaults`

Now, uTorrent server does not currently support SSL, so I'm using a utility called stunnel to open up an HTTPS port that will forward to the local port that uTorrent is listening on.

Firstly, we're going to need a keypair for SSL:  
`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/stunnel/stunnel.key -out /etc/stunnel/stunnel.crt`

To configure stunnel, I copied the example configuration, removed all of the services, added my own (below), and set the cert and key directives to point to the files I just created.
```
[utorrent]
accept = 443
connect = 8888
```


So now https://Tandy400/gui will load up the uTorrent gui over an HTTPS connection. We also want to close off the unencrypted uTorrent page from the outside world, which we do both by not allowing port 8080 through the firewall, and limiting access to 127.0.0.1 from within the web gui.

### BTSync
For the BTSync configuration, I wanted to leave the web server enabled for ease of use, but it's not the sort of thing that I'll need to manage a lot, so I bound it to the local interface, thus requiring an SSH tunnel to connect.  
/etc/btsync/simple.conf:  

```
{
        "device_name": "Tandy400",
        "listening_port" : 0,                   // random port
        "storage_path" : "/var/lib/btsync",
        "check_for_updates" : false,          
        "use_upnp" : false,
        "webui" :
        {
                "listen" : "127.0.0.1:8888",
                "login" : "jimbo",
                "password" : "totally_the_best_password_ever"
        }
}
```

### Dropbox

To get dropbox going, you just have to run `dropbox.py start -i` to install the daemon, and the first time that you successfully start the daemon, you'll be provided with a URL to put in to your browser to link the dropbox CLI client to your account. For my own setup, I didn't want my dropbox folder to be in my home directory, so I created a symlink to /media/disk2/Syncs/Dropbox with `ln -s /media/disk2/SyncsDropbox ~/Dropbox`.
It's pretty much that simple.

### System Monitoring

To monitor my system, I'm using a tool called [phpsysinfo](https://codeload.github.com/phpsysinfo/). It's dead simple to get going- just grab the tarball, extract it to your web directory, and change a couple configuration directives in the .ini file **if you want**.


### Firewall Configuration

Finally, it's time to manage the firewall. Apparently Ubuntu changed things around since the last time that I actively used it, so instead of just dumping some iptables rules into a file, we get to use a little utility called ufw.
Opening up the ports I need couldn't be too much simpler:

```
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
```

