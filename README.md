# Using Raspberry Pi as a Dedicated Ares 2 Enyo Nodejs Server #

I ordered a [Raspberry Pi](http://www.raspberrypi.org/) from the first supplier in England back in February 2012. It was on back order for months and in April I was finally informed they were arriving *soon*. In June I received an email stating 8 weeks shipping, my order was FINALLY being processed. Apparently my order got misplaced, because in September I contacted the company and they admitted nothing, simply stated it was being processed. By the time the RPi (Raspberry Pi) arrrived, I kind of forgot why I ordered it in the first place. So I threw it in a drawer for later, thinking I might set up a media server or something.  

Then I wondered if it's powerful enough to be a dedicated [Ares 2](https://github.com/enyojs/ares-project) server. Ares is an open source, web based Enyo UI  tool, allowing you to drag and drop widgets, wire up events and actions, [here's a demo](https://www.youtube.com/watch?feature=player_embedded&v=qQkzUDtiC-I).

This repository details the steps I took to create my server setup. [You can start byt looking at the wiki here.](https://github.com/pcimino/raspberry-pi-ares/wiki)  

The [Disk Images](https://github.com/pcimino/raspberry-pi-ares/tree/master/images) provide you with installable images to help you setup your Raspberry Pi (RPi).

## Follow Up ##
Disappointing... the RPi's 512 MB isn't enough to load Ares. However this whole tutorial isn't a waste. You can follow the steps and use a Debian Linux computer.   

Also, I did get the RPi to run as a VNC server and Node server for a simple site on a 2 Gig SD Card. So I'll leave this repository up, could prove useful to anyone trying something similar.



