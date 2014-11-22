#First time Raspberry setup#
##without screen / display and keyboard##
##using Mac Os X - Yosemite##

You just got the brand new update version of Raspberry Pi: Raspberry Pi model B+. And now what?

As a teacher, or anybody else without computer skills (like me), you need to setup and perform the Os installation.

This guide is made for you. No blah blah bla, no bullshit. Nice, and simple.

If you use windows, I suggest you to check out [Meltwater's guide](http://pihw.wordpress.com/guides/direct-network-connection/) For linux users, the following guide is quite similar to mac. In this example, it is used a Macbook Pro 13" mid2010 with Mac Os Yosemite (10.10), a graphical SDcard builder[Apple-Pi-Baker v1.6 , 3.6MB released on June 26, 2014.](http://www.tweaking4all.com/hardware/raspberry-pi/macosx-apple-pi-baker/), Iphone power supply and a wirelless-router.

What you need:

1.  5V microUSB power supply. [According Official website](http://www.raspberrypi.org/help/faqs/#powerReqs), Model B+ boards are slightly more power efficient and its circuits requires between 600-2000mA (0.6-2.0A). Electric-current requirements relies on what you connect to it. As [reference](http://www.raspberrypi.org/help/faqs/#powerReqs), 1200mA (1.2A) are enough for most applicattions, including this one!
2.  a network cable
3.  a laptop or computer with ethernet port avaliable (RJ-45). If your internet access is done via cable, in this case you need an extra ethernet card.
4.  a microSDcard at least class 4 with minimum 8GB
5.  a copy of Raspbian downloaded at [official Raspberry website](http://www.raspberrypi.org/downloads/).
6.  setup your SSH conectivity, on your laptop/computer. Since you use a Mac, you are ready to go.
7.  Optional: In this guide, I used a wireless-router and attached the Raspeberry Pi. However, the router is not needed.

Step1: Preparation
------------------

Insert the microSD card into laptop. Now download and install any of these graphical interface: [ApplePi-Baker(free, closed-source, 3.6MB](http://www.tweaking4all.com/hardware/raspberry-pi/macosx-apple-pi-baker/) or [PiWriter(free, open-source)](http://sourceforge.net/projects/piwriter/), or [Pi Filler](http://ivanx.com/raspberrypi/).

Open the choosen graphical interface card builder and following the procedures. If You choose Apple-Pi-Baker:

-   Select the microSD card (step 1 as shown in the figure bellow), and hit "Prep for NOOBS" button (step 2 as shown in the figure bellow).
-   After wait a little bit, its time to build the image into microSD card. Click "Restore Backup" button (step 3 as shown in the figure bellow) and choose the image you saved(step 4 as shown in the figure bellow).The build process should begin.
-   When it's done, remove graphical interface SDcard buider, you don't need it anymore.
-->![alt text](https://github.com/toshioe10/raspberrypi_on_mac_using_yosemite_headless/blob/master/img/Apple-Pi-Baker-steps.png)
Step2: Edit installation files
------------------------------

1.  Open the SDcard, copy the `cmdline.txt` file and rename it to `cmdline.normal`
2.  Launch terminal and type `ifconfig` command to get your laptop IP address. In my case, it is `192.168.2.102`
3.  Open cmdline.txt and give to the raspeberry a local ip address, modifying just the last number's sequence, someting like:
   `ip=192.168.1.XXX`, where XXX will be the Raspberry's IP address you just gave.
4.  Save it, make one more copy, and rename it to `cmdline.direct`
5.  Remove SDcard from laptop / computer and insert into Raspberry Pi.

Step3: Making the first remote connection
-----------------------------------------

1.  On Mac, go to System Preferences \> Sharing and check the following options (You migt be asked for password):
    -   File Sharing
    -   Internet Sharing

2.  Attach the network cable into wireless router and Raspberry Pi
3.  Now it's time to power UP!
4.  Wait a little bit, something like 30 seconds, launch Terminal (on Mac) and type `ssh -X` command, wich has thefollowing syntax: `ssh -X user@hostname`. In our example, just type and follow the instruction given by prompt:

    > `              ssh -X pi@192.168.2.104         `
    >  When asked for password just type
    >  `             raspberry         `

It's done! Now you are connected to Raspberry Pi, and from here you are ready to go, installing [VNC](http://www.raspberrypi.org/documentation/remote-access/vnc/), setup internet config and so on!

Optional
--------

If you want to setup the internet config:

`$ sudo nano /etc/network/interfaces`

Delete its contents, and simply copy and paste:

    auto eth0 
    iface lo inet loopback 

    allow-hotplug wlan0 
    iface eth0 inet dhcp 
    address 192.168.2.102 # Static IP you want 
    netmask 255.255.255.0 
    gateway 192.168.2.1   # IP of your router

    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
    iface default inet dhcp

*(ctrl-X, then type Y to quit and save)*

That's it. Maybe a reboot is required. Now the internet connection should be working. Congratulations!. If you want to setup a WiFi connection, you are almost done.

Configuring WiFi connection
---------------------------

This is not a step by step guide, just some hints that could be helpful. From previous wired connection it could be useful install [VNC](http://www.raspberrypi.org/documentation/remote-access/vnc/).

Open up the wpa\_supplicant.conf file in the editor.

`$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

Just add the following.

    network={ssid="YOUR_NETWORK_NAME"psk="YOUR_NETWORK_PASSWORD"proto="See bellow"key_mgmt="???????See bellow"pairwise="???????See bellow"auth_alg=OPEN}

The other parameters are network specific, I can't tell you what you need. If you boot Raspbian to desktop, you can launch the wpa\_gui (WiFi config) application and click 'Scan'. You'll find a list that has your network with all flags you need. To do this on a RPi, disconnect your keyboard and connect your dongle once the scanning list is open.

-   `proto` could be either `RSN` (WPA2) or `WPA` (WPA1).
-   `key_mgmt` could be either `WPA-PSK` (most probably) or `WPA-EAP` (enterprise networks)
-   `pairwise` could be either `CCMP` (WPA2) or `TKIP` (WPA1)
-   `auth_alg` is most probably `OPEN`, other options are `LEAP` and `SHARED`

Again: Wifi connection setup is just some hint. If you want to install VNC on Raspberry side:`sudo apt-get install tightvncserver`, and you should also install on your laptop, the client version.

Have fun!
