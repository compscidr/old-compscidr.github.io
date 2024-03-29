---
layout: default
title: "Real Mesh Networking and Limitations of Opportunistic Delay Tolerance, Hastily Stitched Together Tech"
published: true
featured-image: ""
featured-alt: ""
categories: [Wireless Networks]
tags: [Wireless Mesh Networks, Delay Tolerance, Wi-Fi Direct, Wi-Fi, Multipeer]
---

There's quite a few apps and companies working on wireless mesh networks. However very few are building what I'd consider to be a true mesh. Many do not care about building the actual underlying connectivity between devices and those who do are often oversimplifying the problem by stitching together things in a very ad-hoc, unplanned manner.

First, and the worst offenders are those who don't care about connectivity at all. They just assume that devices will send using a variety of underlying technologies such as Wi-Fi and Bluetooth, whenever devices happen to be near enough to do so within one hop distance. This is essentially just a delay tolerant heterogeneous network - not a mesh capable of any type of decent performance. Messages and content is cached in devices until it is ready to be forwarded along to the next device or into the Internet.

The next best, and ones which are closer to being meshes are those which stitch together Bluetooth, Wi-Fi, even Wi-Fi direct, Wi-Fi p2p and Multipeer, but don't consider the underlying topologies and self-organizational considerations of the network. While these are a better step in the right direction, there is much room for improvement. The lack of planning at this stage often means that these networks will not scale. They haven't been designed to operate routing protocols on them, they often have no mechanism to exchange neighbour information between clusters of nodes and are built on technologies designed for discovering a limited number of nearby devices.

The stuff we're working on at Left is a truly scalable wireless mesh network - built to discover a large number of nearby devices, to exchange topology information from Bluetooth devices to Wi-Fi devices and intelligently select when to use these connections or when to use the Internet. More details to come.

Originally appeared on Medium: [Real Mesh Networking and Limitations of Opportunistic Delay Tolerance, Hastily Stitched Together Tech
](https://medium.com/@compscidr/real-mesh-networking-and-limitations-of-opportunistic-delay-tolerance-hastily-stitched-together-1ba94595a33b)
