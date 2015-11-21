---
layout: post
title:  Setting elasticsearch with ubuntu-server
date:   2015-11-21 16:11:00
categories: programming ops elasticsearch
tags: server ubuntu es elasticsearch wlan
---

In this post I will be writing steps to set up an ubuntu server to run elasticsearch on it.

## Setting up ubuntu server
You will need a usb at least 4 gb in size.
 
1. Download ubuntu server from [here](http://www.ubuntu.com/download/server).
2. Create a bootable usb stick with the server iso using the steps from [How to create a bootable USB stick on Ubuntu](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-ubuntu)
3. Use the usb to boot the laptop in which you want to setup the server
4. Install the ubuntu server as explained in [this askubuntu answer](http://askubuntu.com/a/340966/78177).
5. In between you might get following error:
    
    ```
    Your installation CD-ROM couldn't be mounted. This probably means
    that yhe CD-ROM was not in the drive. If so you can insert it and try 
    again. 
    
    Retry mounting the CD-ROM? 
    <YES>                          <NO>
    ```
    
    The error is coming because the setup needs the `ubuntu server iso` to be mounted to `/cdrom`. There are many solutions suggested on web (like [this](http://ubuntuforums.org/showthread.php?t=1750464), [this](https://pricklytech.wordpress.com/2013/04/21/ubuntu-server-unattended-installation-custom-cd/)) which were suggesting to mount the iso to `cdrom` manually. But nothing worked for me except [this silly answer](http://askubuntu.com/a/614902/78177).
6. After this restart the server and login.

## Setting up network

There are many ways suggested on web like [this](http://askubuntu.com/questions/294257/connect-to-wifi-network-through-ubuntu-terminal), [this](http://askubuntu.com/questions/16584/how-to-connect-and-disconnect-to-a-network-manually-in-terminal), [this](http://askubuntu.com/questions/138472/how-do-i-connect-to-a-wpa-wifi-network-using-the-command-line), but what worked for me is [Ubuntu 14.04 Server - WiFi WPA2 Personal](http://askubuntu.com/a/650369/78177).
 
Also in case you get confused `router-ssid` is the name of the router network you want to connect to.

## Setting up ssh server

Follow [Install SSH server on Ubuntu Linux](http://www.htpcbeginner.com/install-ssh-server-on-ubuntu-1204/).

## Setup Java

```
cd /opt/
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u51-b16/jdk-8u51-linux-x64.tar.gz"
tar xzf jdk-8u51-linux-x64.tar.gz
apt-get install chkconfig
update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_51/bin/java 3
```

ref: [How to Install JAVA 8 (JDK 8u66) on CentOS/RHEL and Fedora](http://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/#)

## Setup elasticsearch

1. Getting es
    
    ```
    cd
    wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.7.1.zip
    apt-get install unzip
    unzip elasticsearch-1.7.1.zip
    rm *.zip
    cd elasticsearch-1.7.1/
    ```
2. Configure es
    
    ```
    apt-get install vim
    vi ~/elasticsearch-1.7.1/config/elasticsearch.yml
    #cluster.name: elasticsearch
    #node.name: "es-node2"
    #script.inline: on
    #script.indexed: on
    ```
3. Setting up templates
    
    ```
    Add templates:
    mkdir ~/elasticsearch-1.7.1/config/templates
    vi ~/elasticsearch-1.7.1/config/templates/message_template.json
    # Copy template 
    ```
4. Installing plugins
    
    ```
    Install kopf plugin:
    ~/elasticsearch-1.7.1//bin/plugin --install lmenezes/elasticsearch-kopf/1.5.7
    ```
5. Starting es: `/bin/elasticsearch`

## Make es aut start on boot
Again there are many options to do this using [upstart](), rc.d, rc.local, cron as explained [here](http://stackoverflow.com/questions/7221757/run-automatically-program-on-startup-under-linux-ubuntu)
Add `/home/aapa/softwares/elasticsearch-1.7.1/bin/elasticsearch` to the `/etc/rc.local` file as explained in [How to run scripts on start up?](http://askubuntu.com/a/1199/78177).

## Make system don't go to sleep on closing lid
Follow [Ubuntu Server 13.10 now goes to sleep when closing laptop lid](http://askubuntu.com/a/361087/78177)

## Switch off the screen
Follow [Turn off monitor using command line](http://askubuntu.com/a/62861/78177)

## Make the system auto startable

As I am using my old laptop to make the server, it doesn't have much battery backup. In case the system shutdowns due to any reason be it no power, I want it to start automatically. 
 
Right now I am unabel to find any solution for this, but will update soon. The closest resource I found is: [Automatic Reboot After Power Failure](http://askubuntu.com/questions/111907/automatic-reboot-after-power-failure)


## References

1. [How to create a bootable USB stick on Ubuntu](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-ubuntu)
2. [How do I install Ubuntu Server (step-by-step)](http://askubuntu.com/a/340966/78177)
3. [Installing 11.04 server via USBstick](http://ubuntuforums.org/showthread.php?t=1750464)
4. [Ubuntu server – unattended installation (custom cd)](https://pricklytech.wordpress.com/2013/04/21/ubuntu-server-unattended-installation-custom-cd/)
5. [Ubuntu Server 14.04.2 install error: can't umount /media](http://askubuntu.com/a/614902/78177)
6. [Connect to WiFi network through Ubuntu terminal](http://askubuntu.com/questions/294257/connect-to-wifi-network-through-ubuntu-terminal)
7. [How to connect and disconnect to a network manually in terminal?](http://askubuntu.com/questions/16584/how-to-connect-and-disconnect-to-a-network-manually-in-terminal)
8. [How do I connect to a WPA wifi network using the command line?](http://askubuntu.com/questions/138472/how-do-i-connect-to-a-wpa-wifi-network-using-the-command-line)
9. [Ubuntu 14.04 Server - WiFi WPA2 Personal](http://askubuntu.com/questions/464507/ubuntu-14-04-server-wifi-wpa2-personal)
10. [Install SSH server on Ubuntu Linux](http://www.htpcbeginner.com/install-ssh-server-on-ubuntu-1204/)
11. [How to Install JAVA 8 (JDK 8u66) on CentOS/RHEL and Fedora](http://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/#)
12. [Scripting](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting.html)
13. [Index templates](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-templates.html#config)
14. [lmenezes/elasticsearch-kopf](https://github.com/lmenezes/elasticsearch-kopf)
15. [Run automatically program on startup under linux ubuntu](http://stackoverflow.com/a/7221787/742173)
16. [Initialize Script on Startup](http://askubuntu.com/questions/256025/initialize-script-on-startup)
17. [How to run scripts on start up?](http://askubuntu.com/questions/814/how-to-run-scripts-on-start-up).
18. [Ubuntu Server 13.10 now goes to sleep when closing laptop lid](http://askubuntu.com/questions/360615/ubuntu-server-13-10-now-goes-to-sleep-when-closing-laptop-lid)
19. [Turn off monitor using command line](http://askubuntu.com/questions/62858/turn-off-monitor-using-command-line)
20. [Automatic Reboot After Power Failure](http://askubuntu.com/questions/111907/automatic-reboot-after-power-failure)