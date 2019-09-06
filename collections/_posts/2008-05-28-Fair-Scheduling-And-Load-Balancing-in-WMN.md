---
layout: default
title: "Fair Scheduling & Load Balancing in Wireless Mesh Networks (WMN)"
published: true
featured-image: "wmn.jpg"
featured-alt: "Example of a Wireless Mesh Network"
categories: [Computer Science, Research, Wireless Networks]
tags: [Balancing, Fair, Load, Mesh, Networks, Research, Scheduling, Simulation, Wireless]
---
My research is becoming more focused as of late towards the area of fair scheduling and load balancing in Wireless Mesh Networks. Earlier this week I gave a talk in our wireless research group at Guelph on WMN: Fair Scheduling and Load Balancing which I will make available at the bottom of the post.

The presentation gave a background on why load balancing and scheduling are important in WMNs. Additionally a survey on the current problems that I find interesting in the area was presented. In case you don't want to get the presentation / you don't have MS Office here are the main points from the presentation:

## Why is Fair Scheduling Important in WMN:
* Starvation & Unequal Quality of Service (Qos) caused by "greedy" flows or proximity from Gateways which allow other traffic to be ignored or serviced unequally
* In commercial applications of WMNs customers pay equal amounts and expect equal service
* In many solutions that exist today, we cannot have Fairness and Throughput at the same time

{% include featured-image.html %}

## Why is Load Balancing Important in WMN:
* Multiple path redundancy is an important trait of WMN however instead of traffic being spread around the multiple links, often the same links become overused and congested while other links are barely used
* Load Balancing tries to solve this problem using at Mesh Routers, Gateways, and sometimes Links to attempt to spread the traffic around the network (although some papers claim that Load Balancing on the links isn't effective)

## Current Problems in Fair Scheduling and Load Balancing in WMN:
* Assumptions in some papers: single hop networks only, limited or no mobility (of the Mesh Routers), fixed topology (Mesh Routers cannot be added or removed and the topology is known at all times)
* Some papers assume that uplink and downlink scheduling can be treated the same when it may be beneficial to treat them independently
* Distributed Vs Centralized Scheduling & Load Balancing
* Future work on many papers cite experimentation with different metrics, optimal locations and number of gateways etc.
* Thorough investigation of load balancing at the gateways, links or mesh routers

If after all this you would still like the presentation, you can download it from the link below:

Download:

[Fair Scheduling and Load Balancing in Wireless Mesh Networks by Jason Ernst. May 26th 2008.](/assets/presentations/fair-scheduling-load-balancing.ppt)
