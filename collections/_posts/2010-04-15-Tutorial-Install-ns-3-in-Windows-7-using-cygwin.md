---
layout: default
title: "Tutorial: Install ns-3 in Windows 7 using cygwin"
published: true
featured-image: "cygwin-ns3.jpg"
featured-alt: "ns3 on windows 7 with cygwin"
categories: [Computer Science, Featured, Simulation, Tutorials, Wireless Networks]
tags: [802.11, 802.11s, cygwin, NS-3, NS-3.10, NS-3.7.1, Simulation, Windows 7]
---
Ns-3 is one of the most popular simulation tools for network simulation. See the website: [http://www.nsnam.org/](http://www.nsnam.org/). It is the successor to the popular ns-2 simulator. The tool is written in c++ / python, but I manage to get by using mostly only c++ (as opposed to ns-2 which uses c/c++ and tcl). One major difference between ns-3 and ns-2 is that this version has been designed for wireless network simulation from the ground up. (There is support for many types of 802.11 networks including 802.11s mesh networks which interest me).

The following instructions explain how to install the latest stable version of ns3 (~~as of April 14th, 2010~~ as of March 22nd 2011 with ns-3.10) on windows 7 with cygwin. It should work with newer releases as they come out, but if not leave me a message and I will update the instructions. I will try to include any common problems I have encountered, or those of users who comment on the post experience in order to make your install easier.

Step 1:
[Download & install cygwin](http://cygwin.com/install.html). (note: I install it in the recommended directory "{System Root}/cygwin/"). Make sure you select "install" for python and devel. This will ensure you have python and gcc/g++ as well as all of the dependencies. You can do this by clicking on the word default in the list of packages. I left all of the other package options on default for the install.

Step 2:
~~Download ns3.7.1~~Download the most recent version of ns3 from [their website](https://www.nsnam.org/releases/) (you will likely want the all-in-one version if you are just starting out. Save the file to "{System Root}/cygwin/home/{your-username}". Alternatively, you can try downloading the file directly within cygwin using something like "wget http://http://www.nsnam.org/releases/" where filename is the name of the release you wish to install.

Step 3:
Start cygwin. At the cygwin prompt type: "tar xvf ns-allinone-3.7.1.tar.bz2" (or whatever the filename is depending on the version you downloaded). This will unpack the ns3 archive so you can use it.

Step 4:
After it has unpacked, change directory to the new ns3 directory with the following: "cd ns-allinone-3.7.1". Then build ns3 by typing the following: "```./build.py```". You can probably grab a coffee or tea while it compiles.

{% include featured-image.html %}

Step 5:
You should be good to go. You can validate the install by changing directory again to the ns-3.7.1 directory ("```cd ns-3.7.1```") and running "```./test.py```"```. It will let you know with a PASS / FAIL for each test case. You can now edit the files yourself. The scenario files are located in "" and the source files are located in "". Good luck!

Note: For those getting remapping errors, I have finally had the same trouble. This is what worked for me: Open command prompt, change to your /cygwin/bin directory. Type: 'ash'. Then type '/bin/rebaseall'. You can now try cygwin again and it may work. If it still does not work (this happened to me), you can apply ~~[this fix](http://blog.brev.name/2010/09/nodejs-on-windows-7-under-cygwin.html)~~ to the rebaseall script. (you can edit it using some tool like notepad within windows).
