---
layout: default
title: "A Frustrating Experience with Chromium OS"
published: true
featured-image: "partitions.png"
featured-alt: "Example of the partitions"
categories: [Misc, Tutorials]
tags: [2010, Chromium OS, Google, GUID, MBR, Multiboot, Operating System, Partition, Grub 2, Ubuntu 10.04, Windows 7]
---

Recently I reinstalled my laptop and was hoping to add chromium onto my multiboot setup. I can get it working with the standard USB key approach that is recommended on all of the guides, however it seems like moving it to the hard drive is a completely different story. When it is compiled from source and put on the USB key, the partitioning scheme is GUID, and I use the older MBR scheme. It seems to me the only way to get it to work together is either to use a full GUID partioning setup or use some weird hybrid or mixed scheme. From what I've read on other blogs, it doesn't seem particularly easy to get Windows to work with GUID. Also when you look at what gets created on the USB disk itself, its a mess of many partitions, and I'm not particularly fond of that. Perhaps this is an artifact of the GUID scheme since I'm not very familiar with it, so maybe someone can point me in a direction on how to proceed. For now I've given up and will wait and hope that Google will eventually release it in a way that is easy to add to MBR :S Until then I'll just grudgingly use the USB version since I don't want to dedicate my entire laptop hdd to using chromium os.

*note* I've also tried to the hexxeh version of chromium, but for some reason it won't boot on my laptop, and I also prefer to be able to compile from source rather than using a pre-built image.

## What I tried (WIP Post)

I have been wanting to play around with chromium for the past while, but also use the same computer with a multi-boot setup containing Windows 7, Ubuntu 10.04 and Backtrack 4. I am also not fond of having to keep Chromium on a USB key to use it, so there must be some way to get all of them to work together. In my previous setup, I have Win7 on the first partition (I do not allow it to create that stupid 100mb partition at the start of the drive), and then the next partition is an extended partition containing the rest of the OS'es. Inside the extended partition I have Ubuntu 10.04, then Backtrack 4 and then two partitions for Chromium since this seems to be the only way it will work for now. I also have some free space at the end in case I want to add more later :P

{% include featured-image.html %}

For some reason the hexxeh builds that seem to be popular don't want to boot on my Gateway laptop, and the compiled source does, so that is why I am using this method. Also for this install, we are assuming the x86-generic architecture for the Chromium install.

This is what I did to get the OS to compile under Ubuntu 10.04:

First make sure your Ubuntu is the most up-to-date. For some reason the clean install would not compile correctly and I got some errors until I used the most recent stuff:
```
sudo apt-get update
sudo apt-get upgrade
```

Then get the dependency script made by Google which should get everything you need:
```
wget http://src.chromium.org/svn/trunk/src/build/install-build-deps.sh
chmod +x install-build-deps.sh
sudo ./install-build-deps.sh
```

And some extra tools to get the source from Google:
```
sudo apt-get install git-core subversion
svn co http://src.chromium.org/svn/trunk/tools/depot_tools
export PATH=`pwd`/depot_tools:"$PATH"
```

Now we get the source itself (we will put it in home since the compilation depends on this later):
```
cd ~
mkdir chromiumos
cd chromiumos
gclient config http://src.chromium.org/git/chromiumos.git
gclient sync
```

The rest of the instructions build the source, if you want more explanation, see the original Google document.
```
cd ~/chromiumos/src/scripts
./make_chroot

cd ~/chromiumos/src/scripts
./enter_chroot.sh

cd ~/trunk/src/scripts
./setup_board --board=x86-generic

cd ~/trunk/src/scripts
./build_packages --board=x86-generic

./build_image --board=x86-generic
```

If you have no errors, you can exit chroot environment and transfer to usb drive to test.
```
exit
./image_to_usb.sh --from=~/chromiumos/src/build/images/x86-generic/SUBDIR --to=/dev/USBKEYDEV
```

where ```SUBDIR``` is the subdirectory created by build_image, and ```USBKEYDEV``` is the device for the USB key.

If it boots up into your USB key we can now try to get it to work on the hdd.

Sources:
* [http://chromeos.hexxeh.net/wiki/doku.php?id=multiboot](http://chromeos.hexxeh.net/wiki/doku.php?id=multiboot)
* [http://sach33.com/2010/01/05/howto-dualtriple-boot-windowsubuntuchromium/](http://sach33.com/2010/01/05/howto-dualtriple-boot-windowsubuntuchromium/)
* [http://neosmart.net/forums/showthread.php?t=5291](http://neosmart.net/forums/showthread.php?t=5291)
* [http://groups.google.com/group/chromium-os-discuss/browse_thread/thread/7e3838a2b3ab86d7/c95c25990dda1999](http://groups.google.com/group/chromium-os-discuss/browse_thread/thread/7e3838a2b3ab86d7/c95c25990dda1999)
