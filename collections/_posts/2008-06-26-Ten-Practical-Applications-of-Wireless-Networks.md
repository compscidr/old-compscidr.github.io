---
layout: default
title: "Ten Practical Applications of Wireless Networks"
published: true
featured-image: "iot-smarthome.png"
featured-alt: "IoT SmartHome"
categories: [Computer Science, Misc, Wireless Networks]
tags: [Applications, Wireless, Networks]
---

Wireless Networks have become very popular in recent years and research in the area is very active. It is one of my main research interests while studying for my M.Sc. in Applied Computing at the University of Guelph so I thought I would write a bit about what I think are some of the most useful or interesting applications of wireless networking both now and in the future.

## 10. Smart Home
Everything in your house could be controlled by computers and wireless technology. The idea is not new but with wireless technology we have today it becomes even more possible.

One example of this is at IBM where they have a working kitchen that has counter-tops which can detect the type of food you place on them. This information can be then used to give a list of recipes for the various ingredients, suggest cooking times for an automatic microwave / stove or keep track of expiry dates for products. The items in the fridge can be listed without even opening it via an lcd screen on the front which can also double as a way to access the internet for recipes, to watch tv while cooking or ordering more food. Additionally, since it can be hooked up to the internet, the stock in the fridge can be queried from anywhere in the world (say at work) where you can have groceries delivered to your house ready for cooking later that night.

Similarly, other areas of the household could be controlled and manipulated using wireless technologies. Other examples are wireless security cameras, health care applications for patient monitoring, voice communications and more.

{% include featured-image.html %}

## 9. Wireless Agriculture
A key technology used in the above example of the wireless home is the wireless sensor. Research in this area of wireless networking is also quite popular. Wireless sensor networks have applications from monitoring volcanoes and forest fires to climate change in the oceans. Wireless sensors could also be used in other climate sensitive applications such as farming and agriculture.

For instance, I live near Niagara-on-Lake Ontario, Canada where wine making has become very popular recently. They make a particular type of wine called ice-wine which must be an exact temperature for a certain amount of time before it is harvested in order to qualify as ice-wine. Many times they have to call in their staff in the middle of the night in the winter (because that is when the ideal temperature is usually reached) but find out that the temperature is actually not quite cold enough yet. So all the workers are sent home and paid for three hours of work. If wireless sensors were used instead perhaps some money and time could be saved.

Additionally, many of the wineries have technology which circulates air around the fields to control the temperatures from become to extreme for the grapes to grow how they want. These devices could be controlled via wireless sensors spread across the vineyard as well. Similarly, in other areas of agriculture the same ideas could be applied.

![Wireless Agriculture](/assets/img/mesh-agriculture.gif)

## 8. Wireless Power Transmission

Communication over power lines is because electrical power is just a signal in a different band than the communications signals. So we should be able to do the same thing with wireless networks. People have proposed systems where we generate solar power in space (or on the moon) and use wireless signals to beam the power back down to earth (Since everyone is so concerned about clean energy these days I thought this one would be interesting to add).
![](/assets/img/wireless_power.jpg)

## 7. Wireless people
Sensor network technology has been under development for years and has matured to a great degree in the last few. Imagine a system where every person in the world had a chip that could monitor their vital signs (heartbearts, body temperature etc) and then relay these to a central tracking facility such as a hospital or doctors office).

The units could be powered by the energy from blood pumping inside the body, the energy created from walking around, or possible some kind of solar interface. The data coule be relayed in a multi-hop network through each person to the central facitilities. Over time with enough data collected, computers should be able to predict when certain changes in the data are going to produce a heart attack or an blood clot and medical care could become more pre-emptive in areas where predicability is still a problem.

Additionally, farther along in the future similar technology could be used to communicate with others telepathically, to transmit thoughts, ideas etc to other people (its not that far-fetched, they have video game controllers which allow you to play games using brain signals via a headband strapped around your head)

![](/assets/img/rfid_human.jpg)

## 6 Wireless connected cars.
It is becoming more and more popular to have a pc or a well equipped car stereo in a car these days. It would be nice it you could pull up to your driveway and all the music inside your house automatically synced itself into the car. Additionally, it would be even better if it could make use of a network anywhere (cellular, wifi or anything else it comes into contact with on the road) and sync up at anytime the music files in your house change.

On-top of this, having cars connect to each other with vehicluar mesh networks would be an ideal way to form a mobile communication network where files could be shared, and safety information can be exchanged between cars to avoid accidents, traffic congestion, etc.

![](/assets/img/wireless vehicles.jpg)

## Wireless Hospital
Using wireless technology in a hospital has many uses - patient and inventory tracking, cost savings on laying wires in older hospitals which need to send data around etc. The patient and inventory tracking can be done with existing technologies like RFID. This approach could help keep mentally ill patients in the hopsital or prevent mixups with newborns. Companies like Walmart already use this type of system to control inventory in their warehouses and for shipments (one scan can read every item in a trailer to save time and cut down on human error). The only concern in using this type of system in a hospital is that of privacy. No one wants unauthorized people to track them. Additionally, there are concerns that unauthorized people may also find it easier to locate medications and other items within the hospital for theft with such a system in place for inventory. However if this concerns can be overcome then this system could prove quite useful.

![](/assets/img/body_network.png)

## 4. Remote Weather Stations, Buoys etc

The remote weather systems and buoys could make use of technology from Wireless Sensor networks to conserve energy and transmit data only when necessary to the collection stations. They could run on solar panels and the buoys could make use of the technology that allows energy to be captured from waves. If the deployment of either of these stations or buoys was dense enough the traffic could be relayed from the farther nodes inwards toward a central data collection station. Alternatively, these stations could be combined with delay tolerant networks so that data can be relayed from the stations back to the Internet through people walking past.

![](/assets/img/weather_stations.jpg)

## 3. Wireless Traffic / Transit System (Smart-cars + Smart-cities)

This system could be integrated into transit systems already in place. It could be implemented as a mesh network where routers are placed on buses, trains and other vehicles. The users in the vehicles could gain access to the network via the routers in the vehicles which would then relay the traffic through the network to gateways placed at strategic points in the city.

Also, the routers within the vehicles could be equipped with GPS or some other location technology that could be used to relay tracking information back to a website for transit users to plan their travels. Additionally, routers could be placed at various traffic signals and all of the signals in a city could be coordinated in a central control station.

Furthermore, sensors could be places around the road system to give an idea of traffic congestion patterns and other feedback about road conditions (or even in a farther futuristic scenario, cars could be scanned while driving to make sure they are not carrying bombs or other illicit substances).

![](/assets/img/smart-vehicle-smart-city.jpg)

## 2. Independent Private Wireless Community for File and Information Sharing

This could be used for people who like to trade files who are also worried about privacy concerns or people who are tired of public network traffic shaping (torrent traffic etc). This network would be independent from the internet however it could provide many of the same services to people. It could either be free or be on a subscription basis. (if subscription based it would likely require some kind of infrastructure investment) but if it were free, it could work in an ad-hoc arrangement where users connect together and agree to the terms of the community policies for sharing data. A problem that must be overcome however is some way to prevent the sharing of illicit or illegal content in this manner.

![](/assets/img/tor-network.png)

## 1. Community-Wide Wireless Mesh Network for Internet Access

This network could be deployed cheaply across an entire neighbourhood or even a city. The main benefit of the wireless mesh network is that not every router in the neighbourhood requires a wired internet connection. So there could be some kind of scheme where people who provide internet connections are able to make a small amount of money while people who normally cannot afford access could gain cheap or even free / publicly funded access to the network.

Additionally, if many people provide connections into the network, the capacity of the network increases, theoretically allowing access to more bandwidth than each individual connection could provide (ie One person may make use of bandwidth of 2 or 3 connections at once if the network is not busy congested).

![](/assets/img/community-mesh.jpg)
