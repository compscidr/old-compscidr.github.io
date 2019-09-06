---
layout: default
title: "Java Discrete Event Driven Simulation"
published: true
featured-image: "hexagonal.gif"
featured-alt: "Hexagonal Network Topology"
categories: [Computer Science, Simulation, Wireless Networks]
tags: [Discrete, Event, Simulation, Java]
---

Recently in my Networks course at Guelph they had us create an event-driven
discrete simulation to model both a wireless network and a switch. For the
project I got a hexagonal geometry for the wireless network which was to be
arranged similar to a Manhattan Street Network with each node having at most
six neighbours. Other groups got triangular and square geometries for this
portion.  For the switch I got an 8x8 [Banyan switch](https://en.wikipedia.org/wiki/Banyan_switch).
Other groups got the [crossbar switch](https://en.wikipedia.org/wiki/Crossbar_switch) or
backplane switches.

{% include featured-image.html %}

The first run of the simulation was terrible. There were problems with the
timing in the simulation and nodes which were supposed to be holding packets
in them towards the end of the simulation were actually blocking traffic right
at the start causing an extremely high proportion of packets to be dropped. 
In revision two the problems were fixed and the results were so well done that
the prof accused us of doctoring them (we didn't).

So basically, I thought I would post the code online in case anyone would like
to make use of it.  Right now the simulation does not take into account such
factors as interference and is fairly simplistic however it is good for testing
different network geometries and routing algorithms within them. It could easily
be extended to include more complicated factors such as interference however it
was not necessary at the time of the project. If you are making use of the code
please leave a reference to my name in it.  If you have any questions or would
like some help getting it to work feel free to contact me.

For those who are interested here are some of the results from the simulations:
![Banyan Switch Results](/assets/img/banyan-results.jpg)

![Hexagonal Cell Results](/assets/img/hexagonal-results.jpg)

And for those who would like the code here are the links:

* [Wireless Hexagonal Network Simulation](/assets/code/hexagonal.tar.gz)
* [Banyan Switch Simulation](/assets/code/banyan.tar.gz)
