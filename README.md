# Using Raspberry Pi as a Dedicated Ares 2 Enyo Nodejs Server #

I ordered a [Raspberry Pi](http://www.raspberrypi.org/) from the first supplier in England back in February 2012. It was on back order for months and in April I was finally informed they were arriving *soon*. In June I received an email stating 8 weeks shipping, my order was FINALLY being processed. Apparently my order got misplaced, because in September I contacted the company and they admitted nothing, simply stated it was being processed. By the time the RPi (Raspberry Pi) arrrived, I kind of forgot why I ordered it in the first place. So I threw it in a drawer for later, thinking I might set up a media server or something.  

Then I wondered if it's powerful enough to be a dedicated [Ares 2](https://github.com/enyojs/ares-project) server. Ares is an open source, web based Enyo UI  tool, allowing you to drag and drop widgets, wire up events and actions, [here's a demo](https://www.youtube.com/watch?feature=player_embedded&v=qQkzUDtiC-I).

# Disk Images #
This project includes a few disk images (all based off of the [2012-12-16 Raspian Wheezy image](http://www.raspberrypi.org/downloads)):  
1. Raspian-VNC.img Includes the Watchdog and TightVNC Server
2. Raspian-Node.img #1 plus Nodejs v0.8.x
3. Raspian-Ares.img #1 & #2 plus Ares 2 and Nodejs server startup, auto updates

There are two scripts on the desktop for rebooting and powering down the RPi.  

The passwords on the accounts for these images:  
1. root changeme
2. pi changeme
3. TightVNC Client password: changeme

To change the passwords:  
1. System boots into the default user "pi"  
2. Open a command shell  
3. Type: passwd pi  
4. Follow the prompts to change the 'pi' password  
5. Type: sudo passwd root  
6. Follow the prompts to change the 'root' password  
7. To change the TightVNC Server password, stop the server, delete the password file and restart  
8. sudo /etc/init.d/tightvncserver stop  
8. rm .vnc/passwd  
9. sudo /etc/init.d/tightvncserver start  
10. Follow the prompts  

# Setting up the RPi #
I used the wheezy OS build and followed the [beginner instructions](http://www.raspberrypi.org/quick-start-guide) simply to get things going.

First time buooting the RPi puts you into a configuration screen, which can always be re-run later:  
    `sudo raspi-config`

## expand_ ##

This will set up the disk image to expand form 2 Gig to the capacity of the SD Card. I recommend not doing this initially. I had a few false starts and a bad SD Card. I relized the disk image utility could also be used to back up a disk image. And backing up and restoring a 2 Gig disk is not only fster, but I can put it on a 2, 4, 8, 16 ... Gig SD Card.

This allowed me to save multiple images of my work in progress:
1. Raspberry OS with watchdog
2. OS with watchdog plus Tight VNC
3. 1 & 2 plus Nodejs
4. Etcetera, creating images from the more generic towards the specialized

## Overclocking ##

On the setup screen you can select a number of options, 700 MHz is the default clock speed. Of course I immediately went for the speed increase. 1 GHz is iffy, so I went with 950 MHz. This seemed to cause some overheating and the RPi would simply shut off. With my board, apparently the fastest stable speed is 900 MHz.

## Password ##
The default user account is 'pi', and the default password is supposed to be 'raspberry', but never seemed to work if I manually logged out and tried to get back in. So set it here to whatever you want.  

If you're unfamiliar with Linux, the administrator account is 'root', you can set that password:  
`sudo passwd root`  

## Locale and Timezone ##
These default to Great Britain, so you can set these or ignore them for now.

# Headless Server #
One goal of having a dedicated Ares server is to have it run headless: no keyboard or monitor. These tutorials walk you through setting up a fixed IP address, installing a VNC server and, if you feel it necessary, a tunnel to lock down the server.  
  
[Headless Server](http://www.penguintutor.com/linux/raspberrypi-headless)  
[VNC Server](http://www.penguintutor.com/linux/tightvnc)  

I also made an additional change to the script to increase the VNC display area:  
`sudo vi /etc/init.d/tightvncserver`  
Modify the start command to look like:  
`vncserver :1 -geometry 1800x1000 -depth 16 -pixelformat rgb565`

# Nodejs #
I [followed this tutorial](http://www.raspberrypi.org/phpBB3/viewtopic.php?f=34&t=18775), changed the version to the v0.8.x (tried the v0.9.x builds but trying to make the builds caused constant reboots). Even with overclocking, it may take hours; probably want to kick this off over night, or after you have the headless setup and your VNC'ed in.  
  
If you'd like the script to simplify installing Nodejs, pull it here: 
wget https://
# Git #
Installing Git is much quicker:  
`sudo apt-get install git-core`  

# [Watchdog](http://pi.gadgetoid.co.uk/post/001-who-watches-the-watcher) #

The Raspberry Pi BCM2708 hardware has a watchdog. This can be configured to watch for a heartbeat from an applicationor the OS and force a reboot if necessary. Not always recommended since this can corrupt the files on the SD Card, but desirable for a headless server.

1. Load the kernel module  
sudo modprobe bcm2708_wdog  
2. a. Edit the /etc/modules file:
sudo nano /etc/modules  
b. Add the following line:  
bcm2708_wdog
3. Now configure the watchdog daemon.  
sudo apt-get install watchdog chkconfig  
chkconfig watchdog on  
sudo /etc/init.d/watchdog start  
4. Now configure watchdog:  
a. Edit the configuration  
sudo nano /etc/watchdog.conf  
b. Uncomment the line  
watchdog-device = /dev/watchdog

The watchdog daemon sends a heartbeat every 10 seconds. If this signal is absent, your Raspberry Pi will restart.

# Ares 2 #
## Installing ##
There's no real insatallation, simply pull the repository.
`git clone https://github.com/enyojs/ares-project`  
`git submodule update --init`  

## Boot Process ##
Next I need the Raspberry Pi to boot up, and start the node server in the correct directory for Ares.

# Cron Jobs #

Cron is a *nix utility for scheduling activities (such as running a script every day, every 5 minutes, once a month, etc.) Since I want this server to be out of sight, out of mind, I also want some automated maintenance