---
layout: default
title: "One Year in — our mobile mesh platform is taking shape"
published: true
featured-image: "mesh-visualization.png"
featured-alt: "Each device represents a different geographically separate mesh connecting to the Internet (video at bottom)"
categories: [News, Wireless Networks]
tags: [Wireless Mesh Network, Platform, Android]
---

I’ve almost officially been at Left for a year — where I’ve been working on connecting the next billion people with mesh networks. One of the key insights our company has compared with those launching satellites, drones, and other expensive infrastructure plays is that infrastructure is not required. People all around the world can afford a mobile smartphone. Unfortunately, they cannot afford mobile internet access. We’re looking at the problem slightly differently — rather than connect everyone to the Internet — let’s connect them to each other with the equipment they already own (and wherever we can, also to the Internet). We’re aiming to do this with ~~[Wave](http://wave.io)~~ (we renamed it to [RightMesh](https://rightmesh.io) after the first year) which is the mobile mesh networking platform I’ve been building for the last few months. (note: a mobile mesh network is one where traffic travels from phone to phone to phone and not always through cellular towers or Wi-Fi access points).

![Each device represents a different geographically separate mesh connecting to the Internet (video at bottom)](/assets/img/mesh-visualization.png)

There are a few other companies working on this type of technology, but I think we have a few key things that make our upcoming platform much stronger.

Our platform’s architecture is designed in such a way that multiple network interfaces can be used together, or separately to build a robust mesh network. This means you can use Wi-Fi + BT + Wi-Fi direct + other tech that exists on your phone to communicate. Traffic can failover from one technology to another for reliability, or use more than one at a time for a higher throughput link.

Two or more people near to each other, if they wish, can share each others’ Internet connection. If someone wishes to download a file very fast, and their own Internet connection is maxed out, they can take advantage of the other’s connections to download even faster. As long as at least one node in a local mesh has an Internet connection, the entire mesh can be connected (if that person wishes to share).

We do this by not relying on the operating system to perform the routing. We have built our own mesh network stack on top of the normal networking stack. Any app running our platform can transfer data through any other devices also running our platform (it doesn’t have to be the same app). For instance, a device running an emergency app can send data through another device running a text messaging app, as long as both were using Wave.

There are some platforms which operate a really “broadcast” oriented store-and-forward to every other device type of approach. We don’t do this. We route traffic wherever we can so it only passes through the devices it needs to along the way. We store and forward only when a path has broken in the hope that a recovery is possible.

Further, our platform is focused on making thing easy for people using mesh networking apps. It can automatically manage the Wi-Fi, Bluetooth and other connections so the user is just connected — as easily as joining a Wi-Fi network — they can just join the Wave network.

{% include youtubePlayer.html id=cRLJ8WgkREw %}
One of our earliest proof of concepts

We have currently begun internal testing with our Bangladesh office who have created around 10–15 mesh apps. One of our favourites is a [mesh connected fire alarm](https://www.youtube.com/watch?v=qijoZfdpTKs) that was built during a day long hackathon. We’re planning a public release shortly so anyone will be able to build mesh apps with our library.

{% include youtubePlayer.html id=qijoZfdpTKs %}
A mesh connected fire alarm which notifies everyone on the mesh

Now that the basics of the platform have been created, we are moving towards improving the performance and stabilising before our release. We’re also working on integrating some cool encryption and p2p trust technology before release so that you can be sure that privacy is protected as data travels through other devices. Additionally, we’re refining our documentation and working with developers to make sure that the features we launch with are important and useful for release.

{% include youtubePlayer.html id=G1QZZphbEms %}
Performance testing and debugging with some of our mobile mesh networking tools

We are growing our expertise as well. I did my MSc and PhD in mesh networking and HetNets, but we also just added another PhD who is an expert in mesh network routing. We also added some developers with UX experience to help make the development kit even easier to developers to work with.
