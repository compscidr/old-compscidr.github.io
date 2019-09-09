---
layout: default
title: "Tutorial: Ubuntu 12.04 / 11.04 / 10.10 / 10.04 – Grub 2 with Themes"
published: true
featured-image: "graphical-grub.jpg"
featured-alt: "Ubuntu 10.04 with Graphical Grub Template"
categories: [Featured, Linux, Tutorials]
tags: [Burg, Grub 2.0, Lucid, Maverick, Ubuntu 10.04, Ubuntu 10.10, Ubuntu 11.04, Ubuntu 12.04]
---

In this post, I will show you how to install grub 2 with themes so that you can replace the standard text-based grub menu with something that looks a bit nicer. This tutorial will use code which is under development, so it may be best not to use on an important machine. We will actually replace grub with something called burg, which is a developmental branch of grub.

Its quite easy to do now in Ubuntu 12.04 / 11.04, really just one step:

```
sudo add-apt-repository ppa:n-muench/burg
sudo apt-get update
sudo apt-get install burg burg-themes
```

However if you are using 10.04 or 10.10, follow the instructions below:

First, enable the repository for burg by editing your /etc/apt/sources.list file to include the following for Ubuntu 10.10 (Maverick):

```
deb http://ppa.launchpad.net/bean123ch/burg/ubuntu maverick main
deb-src http://ppa.launchpad.net/bean123ch/burg/ubuntu maverick main
```

or use the following for Ubuntu 10.04 (Lucid)

```
deb http://ppa.launchpad.net/bean123ch/burg/ubuntu lucid main
deb-src http://ppa.launchpad.net/bean123ch/burg/ubuntu lucid main
```

Next, (optional) to remove any warnings about gpg signatures, enter the following commands:

```
gpg --keyserver subkeys.pgp.net --recv 55708F1EE06803C5
gpg --export --armor 55708F1EE06803C5 | sudo apt-key add -
```

Alternatively, if you find the pgp serer gives you an error, you can try this one:

```
gpg --keyserver wwwkeys.stinkfoot.us.pgp.net --recv-keys 55708F1EE06803C5
```

Then we need to update apt and install burg:

```
sudo apt-get update
sudo apt-get install burg-pc burg burg-themes
```

This will prompt you to select several options along the way, so far I've just selected the default options since they seem to be detected from your existing grub install. The important one is to select the correct disk (Note: I've only tested in a non-raid system, so I don't know how it will behave with this setup).

On the next restart, we should see a graphical grub menu, something like this:

{% include featured-image.html %}

By default you will probably get the ubuntu template, and you can change the template by pressing 't' during the grub screen. It should also remember which one was last selected.

Note: I have experienced some problems with a fresh install of ubuntu 10.04 and SLI video cards. Burg's graphics mode seems to confuse ubuntu and it gets stuck on boot. If this happens, you can boot in recovery mode and install the restricted nvidia driver. Then on next boot everything should be fine.

Note 2: Occasionally, when Ubuntu updates, grub may install over your burg installation. In order to get burg back, you need to issue the following command:

```
sudo burg-install /dev/sda
```

where ```/dev/sda``` is the partition you want to install burg onto.

Note 3: You might find you have some extra entries that you want to remove, for example the recovery entries or whatever else. You can either edit the burg.cfg file directly located at ```/boot/burg/burg.cfg``` and or you can edit the files in ```/etc/default/burg``` and ```/etc/burg.d/```. If you use the second choice, you will need to run:

```
sudo update-burg
```

in order to regenerate the new burg.cfg file.

Another cool tip I have discovered instead of having to remove your old kernel entries to prevent a ton from showing up in burg, is to toggle them off by typing 'f'. You can do this is burg itself or in the emulator (burg-emu).

Sources:
https://help.ubuntu.com/community/Burg
http://ubuntuforums.org/showthread.php?p=9231199

http://askubuntu.com/questions/105815/how-to-install-burg-theme-in-ubuntu-12-04-alpha-2
