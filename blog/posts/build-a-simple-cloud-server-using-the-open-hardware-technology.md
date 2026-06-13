---
title: "Build a Simple Cloud Server Using the Open Hardware Technology"
date: "2016-12-02T00:13:37+00:00"
modified: "2017-04-19T13:23:18+00:00"
slug: "build-a-simple-cloud-server-using-the-open-hardware-technology"
author: "Ahmed Lekssays"
featured_image: "../images/build-a-simple-cloud-server-using-the-open-hardware-technology/a421a325-raspberry20pi20supercomputer206.jpg"
categories: ["Computer Science"]
tags: ["cloud computing", "open source", "parallel processing", "raspberry pi"]
original_url: "https://lekssays.wordpress.com/2016/12/02/build-a-simple-cloud-server-using-the-open-hardware-technology/"
excerpt: "Abstract Data storage has become a critical problem that facing computer scientists. Not only is it the huge amount of data; it is also also the problem of safety and privacy because of the revolution of information technology security. In order to find a possible solution to these problems, this pr"
---
## Abstract

Data storage has become a critical problem facing computer scientists. The challenge is not only the sheer volume of data; it is also the problem of safety and privacy, driven by the revolution in information technology security. To find a possible solution to these problems, this project combines open hardware and open source software. It is based on the Raspberry Pi 2, a single-board computer with powerful characteristics (a quad-core CPU, 1 GB of RAM, and 3D Full HD graphics), and on OwnCloud, a file-sharing web script. The main challenge of this project is how to combine eight (or more) Raspberry Pis and make them act as a single cloud server with parallel processing. At present, a web server meeting these requirements costs around $2000, but the equivalent server built with Raspberry Pis costs $500.

We aim to address the following questions:

1. What is the Open Hardware Technology?
2. How can we combine eight Raspberry Pis for parallel processing?
3. How can we build a secure file-sharing environment using OwnCloud?

Because of the cost, only large companies can afford a commercial server. By contrast, the Raspberry Pi server can be built by anyone with modest funds. This simple cloud server is secure because the OwnCloud script is configured with an SSL certificate to encrypt the user's data and prevent man-in-the-middle attacks. Furthermore, a company's employees can share their files privately without using any external drive, which avoids transferring viruses and malware. In addition, the company's files stay hidden from attackers because the server is local; consequently, they are visible only to people connected to the same network as the server. This project combines open hardware and open source software, so it can be developed and updated by developers around the world.

This cloud server is built in two main steps:

1. **The hardware part:** building the server by installing and configuring the Raspbian OS on the first Raspberry Pi, then installing and configuring the MPICH and MPI4PY libraries and applying the same configuration to the other boards.
2. **The software part:** preparing the server to run the OwnCloud script. This is done by installing and configuring the PHP5, APACHE2, and OPENSSL packages, then configuring OwnCloud's database system and admin account.

As a result, the server will have the following characteristics:

- CPU: 7.2 GHz / 32 Cores
- HDD: 512 GB
- RAM: 8 GB
- Graphics: 3D / Full HD 1080p + 8 HDMI Ports

You can build one for about $516.

## Introduction

The Raspberry Pi 2 is a single-board computer that lets us create many interesting projects by programming it to perform specific tasks at a reasonable price. The board was created by the Raspberry Pi Foundation to support the teaching of computer science around the world and to promote open hardware technology. We will work through the method of building a simple cloud server with the Raspberry Pi 2. This cheaper server allows people connected to the same network to store and share their files together in the cloud in a secure way. We will also use the OwnCloud script, a free and open source file-sharing software. Our project is therefore a combination of open hardware and open source software, introducing what is now called the Open Cloud Technology. I believe that open hardware technology can improve the creativity of computer science students.

## Building the Server

### Requirements

To build this simple server, we need the following.

**Hardware:**

- 8 x Raspberry Pi 2
- 1 x switch with 8 Ethernet ports
- 2 x USB 2.0 hubs (2 x 4 ports)
- 8 x SD cards (8 x 64 GB)
- 8 x power cables (Micro USB)
- 10 x networking cables

**Software:**

- OS: Raspbian OS
- Apps:
  - Win32-disk Imager
  - MPICH sources
  - MPI4PY

The challenge of this project is how to combine 8 Raspberry Pis, each of which has the following technical characteristics:

- 900 MHz quad-core ARM Cortex-A7 CPU
- 1 GB RAM
- 4 USB ports
- 40 GPIO pins
- Full HDMI port
- Ethernet port
- 3.5 mm audio jack and composite video
- Camera interface (CSI)
- Display interface (DSI)
- MicroSD card slot
- BroadCom VideoCore IV 3D graphics core

So if we combine just 8 Raspberry Pis, we get a server with these characteristics:

- CPU: 7.2 GHz / 32 Cores
- HDD: 512 GB
- RAM: 8 GB
- Graphics: 3D / Full HD 1080p + 8 HDMI Ports

### Install and Configure Raspbian OS on the First Board (Pi01)

In this step, we install and configure the Raspbian OS on the Raspberry Pi 2. First, write the number of each Raspberry Pi 2 (Pi01, Pi02, Pi03, ...) on its MicroSD card to keep our work organized. Take the MicroSD card (Pi01) and connect it to your laptop. Then download the Raspbian OS from this link:

[Download Raspbian OS](http://www.raspberrypi.org/downloads/) and Win32-Disk Imager by filling out the form below:

[WIN32-DISK IMAGER DOWNLOAD](http://resources.infosecinstitute.com/build-a-simple-cloud-using-the-open-hardware-technology/#download)

After connecting your MicroSD card (Pi01), select it, click Browse, select your Raspbian image, then click Write.

For more details, you can read this documentation by the Raspberry Pi Foundation:

- For Windows: [Windows Documentation](http://www.raspberrypi.org/documentation/installation/installing-images/windows.md)
- For Mac: [Mac Documentation](http://www.raspberrypi.org/documentation/installation/installing-images/mac.md)

Once the Raspbian OS image has finished burning, connect the MicroSD card to your Raspberry Pi (Pi01); this board will control the other boards to create a parallel-processing system. Then connect the power cable, keyboard, mouse, and Ethernet cable. You will see the screen below:

![Raspberry Pi configuration screen](https://i0.wp.com/2we26u4fam7n16rz3a44uhbe1bq2.wpengine.netdna-cdn.com/wp-content/uploads/061015_1122_BuildASimpl1.png)

**Figure 1: Raspberry Pi Configuration Screen**

If it does not appear, type **sudo raspbi-config**. After that, expand the file system, overclock the Raspberry Pi to 900 MHz, change the hostname to **Pi01**, split the memory to 16 MB of graphics, and enable SSH so we can control the board from another laptop (on the same network). Before you reboot the board, enable auto-login by typing **sudo nano /etc/inittab** at the command line to edit the inittab file. Find this line, **#1:2345:respawn:/sbin/getty –noclear 38400 tty1**, and comment it out by deleting the **'#'**, then add this line beneath it: **1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2>&1**. Now you can reboot your board.

#### Configure MPICH

MPICH is an open source software package that lets us create multi-processing communication between computers in a parallel way. You can install it by following these directions:

- ***sudo apt-get update*** (Get the latest packages of the OS)
- ***mkdir mpich3*** (Create a directory named mpich3)
- ***cd ~/mpich3*** (Open this directory)
- ***wget <http://www.mpich.org/static/downloads/3.1/mpich-3.1.tar.gz>*** (Download mpich-3.1.tar.gz because it is the stable version)
- ***tar xfz mpich-3.1.tar.gz*** (Decompress the package)
- ***sudo mkdir /home/rpimpi/*** (Create a directory named rpimpi in the home folder; you need root permissions)
- ***sudo mkdir /home/rpimpi/mpi-install*** (Create a directory named mpi-install in /home/rpimpi; you need root permissions)
- ***mkdir /home/pi/mpi-build*** (Create a directory named mpi-build in /home/pi/)
- ***cd /home/pi/mpi-build*** (Open this directory)
- ***sudo apt-get install gfortran*** (Install the GNU gfortran compiler package)
- ***sudo /home/pi/mpich3/mpich-3.1/configure-prefix=/home/rpimpi/mpi-install*** (Configure the build)
- ***sudo make*** (Determine which part of the program must be recompiled)
- ***sudo make install*** (Execute the Makefile)
- ***nano .bashrc***
  - ***PATH=$PATH:/home/rpimpi/mpi-install/bin*** (Add MPI to the .bashrc boot file)
- ***sudo reboot*** (Reboot the device)
- **mkdir mpi-testing** (Create this directory to test that MPI works for a single node)
- **cd ~** (Change directory and go to /home)
- **cd mpi-testing** (Open this directory)
- **ifconfig** (Your IP will look like 192.168.1.\*)
- **nano machinefile** (Add your IP to it: 192.168.1.\* / edit machinefile)
- **mpiexec -f machinefile -n 1 hostname** (Test MPI)

If the console displays **raspberry pi**, MPI is working.

### Install and Configure MPI4PY

We installed gfortran, a C compiler, and we know the Raspberry Pi is a Python coding environment, so we should install the MPI4PY package to compile \*.py files. You can follow these steps to install and configure MPI4PY:

- **sudo aptitude install python-dev** (Install the Python package)
- **wget<https://mpi4py.googlecode.com/files/mpi4py-1.3.1.tar.gz>** (Download the MPI4PY package)
- **tar -zxf mpi4py-1.3.1** (Decompress the MPI4PY package)
- **cd mpi4py-1.3.1** (Open the mpi4py-1.3.1 folder)
- **python setup.py build** (Build the setup.py file)
- **python setup.py install** (Execute the setup.py file)
- **export PYTHONPATH=/home/pi/mpi4py-1.3.1** (Set the PYTHONPATH variable)
- **mpiexec -n 5 python demo/helloworld.py** (Test whether MPI works for Python)

### Install and Configure Raspbian OS on the Other Boards

After finishing this procedure, we have another image of the Raspbian OS configured for use on the other boards (its size is greater than the original image). We will clone this image to all the other MicroSD cards (Pi02, Pi03, Pi04, ..., Pi08).

We use the same method for all the boards. First, shut down the first Raspberry Pi by typing:

- **sudo poweroff**

Then connect the next MicroSD card (Pi02) to your laptop, burn the new Raspbian image to it using Win32-Disk Imager (as in the previous steps), and repeat for all the MicroSD cards (Pi03, Pi04, ... Pi08). After that, install nmap to manage your network by typing in the terminal:

- **sudo apt-get update**
- **sudo apt-get install nmap**

Before we continue, connect all the boards to the router and power, and insert the MicroSD cards in each one. To configure the network status of each Raspberry Pi, determine your internal IP by typing:

- **ifconfig**
- **sudo nmap -sn 192.168.1.\*** (Scan the subnet for the boards on the network)

We should first connect to all the boards over SSH and apply the same configuration listed in chapter 1.2 (the Raspberry Pi Configuration Screen), changing the hostname to Pi0\* (the appropriate value) using these commands.

You will configure all these Raspberry Pis from Pi01.

- Pi02
  - **ssh pi@192.168.1.3**
  - **sudo raspi-config**
- Pi03
  - **ssh pi@192.168.1.4**
  - **sudo raspi-config**
- Pi04
  - **ssh pi@192.168.1.5**
  - **sudo raspi-config**
- Pi05
  - **ssh pi@192.168.1.6**
  - **sudo raspi-config**
- Pi06
  - **ssh pi@192.168.1.7**
  - **sudo raspi-config**
- Pi07
  - **ssh pi@192.168.1.8**
  - **sudo raspi-config**
- Pi08
  - **ssh pi@192.168.1.9**
  - **sudo raspi-config**
- **mpiexec -n 1 hostname** (Run the test file)
- **mkdir mpi\_test** (Create a test directory)
- **cd mpi\_test** (Open the test directory)
- **nano machinefile** (Create the machinefile so MPI can read the boards)
  - **192.168.1.2** (Pi01)
  - **192.168.1.3** (Pi02)
  - **192.168.1.4** (Pi03)
  - **192.168.1.5** (Pi04)
  - **192.168.1.6** (Pi05)
  - **192.168.1.7** (Pi06)
  - **192.168.1.8** (Pi07)
  - **192.168.1.9** (Pi08)
- **mpiexec -f machinefile -n 8 hostname** (Test that everything is working)

Unfortunately, you will see that only Pi01 works, because we do not yet have permission to read from and write to the other boards. To solve this problem, we create authentication keys by following the commands below.

This step adds the public-private keys to the boards:

- PI01
  - **ssh-keygen** (Generate a new key and save it in /home/pi/.ssh/id\_rsa without a PASSWORD)
  - **cd ~**
  - **cd .ssh** (Open the .ssh directory)
  - **cp id\_rsa.pub pi01** (Copy the key to a new file called pi01)
  - **ssh pi@192.168.1.3** (Connect to Pi02 and do the same thing by following the commands below)
- PI02
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi02**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
  - **ssh pi@192.168.1.4**
- PI03
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi03**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
  - **ssh pi@192.168.1.5**
- PI04
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi04**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
  - **ssh pi@192.168.1.6**
- PI05
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi05**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
  - **ssh pi@192.168.1.7**
- PI06
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi06**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
  - **ssh pi@192.168.1.8**
- PI07
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi07**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
  - **ssh pi@192.168.1.9**
- PI08
  - **ssh-keygen**
  - **cd .ssh**
  - **cp id\_rsa.pub pi08**
  - **scp 192.168.1.2:/home/pi/.ssh/pi01 .**
  - **cat pi01 >> authorized\_keys**
  - **exit**
- Back on PI01: add the other boards to the authorized\_keys list:
  - **scp 192.168.1.3:/home/pi/.ssh/pi02**
  - **cat pi02 >> authorized\_keys**
  - **scp 192.168.1.4:/home/pi/.ssh/pi03**
  - **cat pi03 >> authorized\_keys**
  - **scp 192.168.1.5:/home/pi/.ssh/pi04**
  - **cat pi04 >> authorized\_keys**
  - **scp 192.168.1.6:/home/pi/.ssh/pi05**
  - **cat pi05 >> authorized\_keys**
  - **scp 192.168.1.7:/home/pi/.ssh/pi06**
  - **cat pi06 >> authorized\_keys**
  - **scp 192.168.1.8:/home/pi/.ssh/pi07  
    cat pi07 >> authorized\_keys**
  - **scp 192.168.1.9:/home/pi/.ssh/pi08**
  - **cat pi08 >> authorized\_keys**

## Configure Your File-Sharing Cloud Server

### Download the Needed Packages

Now that we have built our server, we will work on Pi01 because it controls all the other boards. First, we should give it a static internal IP. (As mentioned before, the IP of Pi01 is 192.168.1.2.)

Follow these directions to give it a static IP:

- **sudo nano /etc/network/interfaces** (Edit the interfaces file)

![/etc/network/interfaces file](https://i0.wp.com/2we26u4fam7n16rz3a44uhbe1bq2.wpengine.netdna-cdn.com/wp-content/uploads/061015_1122_BuildASimpl2.png)

**Figure 2: /etc/network/interfaces file**

Then save and exit the file, and restart networking to apply the new configuration.

- **sudo /etc/init.d/networking restart**

After that, check that our OS is up to date.

- **sudo apt-get update**

Once these steps are done, we install:

- **Apache with SSL** (To make our server a web server and secure file transfers with an SSL certificate. The URL of our server will be [**https://192.168.1.2/**](https://192.168.1.2/).)
- **PHP5** (To configure our server to read and host .php files)
- **PHP5 APC** (To increase the speed of page loading)

Run:

- **sudo apt-get install apache2 php5 php5-json php5-gd php5-sqlite curl libcurl3 libcurl4-openssl-dev php5-curl php5-gd php5-cgi php-pear php5-dev build-essential libpcre3-dev php5 libapache2-mod-php5 php-apc gparted**

### Configure the Downloaded Packages

#### Configure PHP5 APC

First, install the apc package:

- **sudo pecl install apc**
- **sudo nano /etc/php5/cgi/conf.d/apc.ini** (Edit the apc.ini file)

Add these lines to the apc.ini file and save it:

**extension=apc.so**

**apc.enabled=1**

**apc.shm\_size=30**

#### Configure APACHE2 and OPENSSL

To configure Apache, first edit the file php.ini.

- **sudo nano /etc/php5/apache2/php.ini**

First, find **upload\_max\_filesize** and **post\_max\_size**, then change their values to 1024 MB to allow users to upload files up to 1 GB.

Second, find **externsion=** and set the value to **acp.so** (**externsion=acp.so**). Save the file and exit.

- **sudo nano /etc/apache2/sites-enabled/000-default**

When this file is open, we need to change AllowOverride from None to All.

- **sudo a2enmod rewrite**
- **sudo a2enmod headers**
- **sudo openssl genrsa -des3 -out server.key 1024; sudo openssl rsa -in server.key -out server.key.insecure;sudo openssl req -new -key server.key -out server.csr;sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt;sudo cp server.crt /etc/ssl/certs;sudo cp server.key /etc/ssl/private;sudo a2enmod ssl;sudo a2ensite default-ssl** (Configure OPENSSL)
- **sudo service apache2 restart** (Restart Apache to apply these configurations)

OPENSSL is important because it protects the user from man-in-the-middle attacks, in which an attacker can steal the user's personal data, such as email, password, and transferred images, on the network. It does so by encrypting the incoming and outgoing connections (see Figure 3 below).

![Man in the middle attacks](https://i0.wp.com/2we26u4fam7n16rz3a44uhbe1bq2.wpengine.netdna-cdn.com/wp-content/uploads/061015_1122_BuildASimpl3.png)

**Figure 3: Man-in-the-Middle Attacks**

### Configure the OwnCloud Script

OwnCloud is an open source, web-based script that lets users store and share files among themselves in the cloud. The important thing about OwnCloud is that it supports all devices (as you can see in Figure 4).

![How OwnCloud works](https://i0.wp.com/2we26u4fam7n16rz3a44uhbe1bq2.wpengine.netdna-cdn.com/wp-content/uploads/061015_1122_BuildASimpl4.png)

**Figure 4: How OwnCloud Works**

To install this script on our server, we first download its package.

- **wget <http://mirrors.owncloud.org/releases/owncloud-4.5.1.tar.bz2>**
- **sudo tar -xjf owncloud-4.5.1.tar.bz2** (Decompress the package)
- **sudo cp -r owncloud /var/www** (Copy the directory to the web root)
- **sudo chown -R www-data:www-data /var/www/owncloud/** (Give the web server permission to access the OwnCloud directory)
- **sudo nano /var/www/owncloud/.htaccess** (Edit the .htaccess file; do the same thing as in the php.ini file and change the upload size to 1024 MB)

Then go to your browser and type:

[**https://YOURIPADDRESS/owncloud**](https://youripaddress/owncloud) (for us, the IP is **192.168.1.2**)

You will be redirected to the setup main page, where you:

1. Enter the admin username.
2. Enter the admin password.
3. Specify the OwnCloud directory location.
4. Select the database system (in our case, SQLite).

This is shown in Figure 5 below:

![The configuration interface of OwnCloud](https://i0.wp.com/2we26u4fam7n16rz3a44uhbe1bq2.wpengine.netdna-cdn.com/wp-content/uploads/061015_1122_BuildASimpl5.jpg)

**Figure 5: The Configuration Interface of OwnCloud**

## Conclusion

To sum up, this cloud was built by combining 8 single-board computers with a file-sharing script. As you can see, if we use open hardware technology well, we can build creative projects at minimal cost. As a result, we built a simple cloud server that helps a company's employees share files without any risk, at the following cost:

- 8 x Raspberry Pi 2 (8 x $35 = $280)
- 1 x switch with 8 Ethernet ports ($25)
- 2 x USB 2.0 hubs (2 x 4 ports) ($10)
- 8 x SD cards (8 x 64 GB) (8 x $25 = $200)
- 8 x power cables (Micro USB) (included in the Raspberry Pi package)
- 9 x networking cables (included in the Raspberry Pi package + 1 cable we buy for $1)

Total: 280 + 25 + 10 + 200 + 1 = $**516**

By contrast, the price of a commercial web server is over $2000.

## References

- Cox, S. (2012). Steps to make Raspberry Pi Supercomputer. Retrieved January 15, 2015, from <http://www.southampton.ac.uk/~sjc/raspberrypi/pi_supercomputer_southampton.htm>
- GNU Fortran. (2014). Retrieved February 2, 2015, from <https://gcc.gnu.org/fortran/>
- Leonard, P. (2012). Parallel Processing on the Pi (Bramble). Retrieved January 27, 2015, from <http://westcoastlabs.blogspot.co.uk/2012/06/parallel-processing-on-pi-bramble.html>
- Linux Foundation Releases 2015 Guide to the Open Cloud. (2015). 23-23. Retrieved January 29, 2015, from <http://www.linuxfoundation.org/news-media/announcements/2015/01/linux-foundation-releases-2015-guide-open-cloud>
- Lynn, H. (2015). Raspberry Pi 2 on sale now at $35. Retrieved January 31, 2015, from [http://www.raspberrypi.org/blog/#raspberry-pi-2-on-sale](http://www.raspberrypi.org/blog/)
- Make Your Own Cluster Computer. (2014). Retrieved January 18, 2015, from <http://www.tinkernut.com/2014/04/27/make-cluster-computer/>
- MPICH Developer's Documentation. (2014). Retrieved January 27, 2015, from <https://wiki.mpich.org/mpich/index.php/Main_Page>
- NMAP Documentation. (2012). Retrieved January 22, 2015, from <http://nmap.org/docs.html>
- Figure 4. OwnCloud Community. (2014). Retrieved January 20, 2015, from <http://static.pcinpact.com/images/bd/news/119388-owncloud-community-4-5-beta.png>
- Figure 5. OwnCloud Configuration Screen. (2012). Retrieved January 20, 2015, from <http://cdn.instructables.com/FZV/0OWA/H8CVGMZ2/FZV0OWAH8CVGMZ2.LARGE.jpg>
- OwnCloud Documentation Overview. (2014). Retrieved January 23, 2015, from <http://doc.owncloud.org/>
- Raspberry Pi Owncloud (dropbox clone) (2012). Retrieved January 20, 2015, from <http://www.instructables.com/id/Raspberry-Pi-Owncloud-dropbox-clone/>
- Figure 1. Raspi-Config Screen. (2013). Retrieved January 20, 2015, from <http://www.blogcdn.com/www.engadget.com/media/2012/08/expandrootfsopt1.png>
- Smith, S. (2012). Logging into a Raspberry Pi using Public/Private Keys. Retrieved January 28, 2015, from <http://steve.dynedge.co.uk/2012/05/30/logging-into-a-rasberry-pi-using-publicprivate-keys/>
- Image Copyright: University of Southampton <../images/build-a-simple-cloud-server-using-the-open-hardware-technology/9e5dbc38-raspberry_20pi_20supercomputer_206.jpg>
