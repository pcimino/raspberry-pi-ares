# Disk Images #
This project includes a few disk images (all based off of the [2012-12-16 Raspian Wheezy image](http://www.raspberrypi.org/downloads):  
1. [Wheezy-VNC.img](http://www.adrive.com/public/wgm7QJ/Wheezy-VNC.zip) Includes the rpi-update tool, git, Watchdog and TightVNC Server  
2.  [Wheezy-Node.img](http://www.adrive.com/public/HRdsmW/Wheezy-Node.zip) #1 plus Nodejs v0.8.19 and some npm modules useful for running an unattended server.  

There are two scripts on the desktop for rebooting and powering down the RPi.  

## The passwords on the accounts for these images:  ##
1. root changeme  
2. pi changeme  
3. TightVNC Client password: changeme  

## To change the passwords: ##
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


[Project Wiki](https://github.com/pcimino/raspberry-pi-ares/wiki)