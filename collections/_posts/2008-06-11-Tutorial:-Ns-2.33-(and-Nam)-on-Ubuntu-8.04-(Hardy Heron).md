---
layout: default
title: "Tutorial: Ns-2.33 (and nam) on Ubuntu 8.04 (Hardy Heron)"
published: true
featured-image: "ubuntu-8.04-update.png"
featured-alt: "Ubuntu 8.04 Update"
categories: [Computer Science, Linux, Simulation, Tutorials, Wireless Networks]
tags: [Ubuntu, 8.04, Guide, Hardy, Heron, HowTo, Install, Linux, Ubuntu, NS-2.33, Wireless, Simulation]
---

Since I have been working with ns2 for the last few months in preparation for my thesis I have decided to write a guide on how to install the most recent version of ns2 on the most recent version of ubuntu (at the time of this writing, Monday June 9th, 2008).

I have found many people already who have had difficulty setting it up so maybe this will be of some help to someone. For this tutorial I am assuming you have installed the most recent version of Ubuntu (8.04). (At the time of writing)

## Step 1: Update Ubuntu

Since it has already been a month or so since Hardy Heron has been released its probably best, if you haven't already done so to update Ubuntu. The easiest way I've found is to go to System>>Administration>>Update Manager. Alternatively, you can enter this into the terminal:

```
sudo apt-get update
sudo apt-get upgrade
```

{% include featured-image.html %}

## Step 2: Download all of the required pieces to make ns2 work
Here is a list of archives required for ns2 to work properly (you can download them by clicking the links or just enter all of the commands in the code sections below for automatic download and untar). I saved each archive to the desktop so I could find each one easily but you could use anywhere you like.

1. [tcl 8.4.19](/assets/code/tcl8.4.19-src.tar.gz)
2. [tk 8.4.19](/assets/code/tk8.4.19-src.tar.gz)
3. [otcl 1.13](/assets/code/otcl-src-1.13.tar.gz)
4. [tclcl 1.19](/assets/code/tclcl-src-1.19.tar.gz)
5. [ns 2.33](/assets/code/ns-2.33.tar.gz)
6. [nam 1.13](/assets/code/nam-src-1.13.tar.gz)

### Step 2.5: Install Ubuntu Packages
Before installing everything else, I have found that it is helpful to get a few packages from the ubuntu repositories or else it wont build correctly. Grab a cup of coffee it might take a while since kdebase-dev is somewhat large. I'm sure there is a way to do this with fewer packages however I know this works. If anyone figures out exactly which packages are required let me know so I can update this.

`sudo apt-get install libx11-dev kdebase-dev`

## Step 3: Install tcl
```
wget http://www.jasonernst.com/assets/code/tcl8.4.19-src.tar.gz
tar xvf tcl8.4.19-src.tar.gz
cd tcl8.4.19/unix
./configure
make
sudo make install
```

## Step 4: Install tk
```
cd ..
cd ..
wget http://www.jasonernst.com/assets/code/tk8.4.19-src.tar.gz
tar xvf tk8.4.19-src.tar.gz
cd tk8.4.19/unix
./configure
make
sudo make install
```

### Step 5: Install oTcl
*Important Note*: The ```./configure --with-tcl=``` portion of the following two code fragments should point to the source files for tcl. If you have been following the guide by copying and pasting the previous commands what we have here will work just fine. Otherwise you will need to change this to point to the correct location of your tcl source files.
```
cd ..
cd ..
wget http://www.jasonernst.com/assets/code/otcl-src-1.13.tar.gz
tar xvf otcl-src-1.13.tar.gz
cd otcl-1.13
./configure --with-tcl=../tcl*/
make
sudo make install
```

## Step 6: Install tclcl
```
cd ..
wget http://www.jasonernst.com/assets/code/tclcl-src-1.19.tar.gz
tar xvf tclcl-src-1.19.tar.gz
cd tclcl-1.19
./configure --with-tcl=../tcl*/
make
sudo make install
```
## Step 7: Install ns2
```
cd ..
wget http://www.jasonernst.com/assets/code/ns-2.33.tar.gz
tar xvf ns-2.33.tar.gz
cd ns-2.33
./configure
make
sudo make install
```

You will likely get alot of warnings for deprecated conversions. Just ignore these or if you really are concerned about them visit the [nsnam troubleshooting](https://nsnam.isi.edu/nsnam/index.php/Troubleshooting#ns-2_not_building_with_gcc.2Fg.2B.2B_4.3.2) page. If you want to make sure your version of ns-2 is working correctly after the install you can run the validation test from within the ns2 source directory. You can do this by entering:
`./validate`

You should see that the test output agrees with the reference output. Congratulations you have a working version of ns-2 installed.

## Step 8: (optional) Install nam

```
cd ..
wget http://www.jasonernst.com/assets/code/nam-src-1.13.tar.gz
tar xvf nam-src-1.13.tar.gz
cd nam-1.13
./configure
make
sudo make install
```

Note: if you get this error:
`error: X11/Xmu/WinUtil.h: No such file or directory`

it may be necesary to do this:

`sudo apt-get install libxmu-dev`

You are now ready to start working with ns2 and nam. If you are like me and working on a new protocol or something you will want to start modifying the ns-2 source code and recompile again so you might want to keep that folder handy. The rest of the source can be safely removed as far as i know. Keep in mind after you have modified the source you will want to do another make install so that when you type ns in your terminal the version you just compiled is used.

## Getting Started with NS2

These are some quick tips to get you started using ns2 if you are a beginner.

All example files are located in `ns/tcl/ex`. You can run these scenarios on ns2 using `ns filename.tcl` The best way to start is probably changing things in these files until you understand what is happening more thoroughly.

The output will usually be a trace file with a similar name: ex) filename.tr. Trace files can usually be viewed with a text-editor program. There are also tools to analyze the trace files and pull stats from them. These may require some tweaking however depending on the format of the trace file. Additionally, a nam output file for visualization may be generated as well. This will usually be named filename.nam. To view the visualization use: `nam filename.nam`

## Useful links
I thought it might be useful to add a section on useful links here since this article has become quite popular. If you would like to suggest a link feel free to contact me.
* [Ns2 Wiki](https://nsnam.isi.edu/nsnam/index.php/Main_Page)
* [Marc Greis' Tutorial](https://isi.edu/nsnam/ns/tutorial/index.html)
* [NS Manual](https://isi.edu/nsnam/ns/ns-documentation.html)
* [NS for Beginners](http://www-sop.inria.fr/members/Eitan.Altman/COURS-NS/n3.pdf) (pdf)
* [Common NS2 Installation Problems](https://isi.edu/nsnam/ns/ns-problems.html)
* [NS2 Wireless and Mobility Modules](https://nsnam.isi.edu/nsnam/index.php/Contributed_Code#Wireless_and_Mobility)
* [NS2 Mailing Lists](https://www.nabble.com/Network-Simulator-ns-2-f15582.html)
* [NS2.33 Wireless MAC and PHY improvements](https://dsn.tm.uni-karlsruhe.de/english/Overhaul_NS-2.php)
