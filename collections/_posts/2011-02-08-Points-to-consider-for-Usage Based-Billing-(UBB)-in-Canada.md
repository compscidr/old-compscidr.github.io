---
layout: default
title: "Points to consider for Usage Based Billing (UBB) in Canada"
published: true
featured-image: "connection-map.png"
featured-alt: ""
categories: [Computer Science, Featured, Miscellaneous, Wireless Networks]
tags: [2011, Canada, ICC 2010, Internet, ISP, Price, Service, UBB, Usage Based Billing]
---

While I make no claims to understand the economics of the agreements between ISPs for forwarding traffic between each other, the point of this article is to provide a unique perspective since I am a graduate student in the networking field. I also briefly outline some potential techniques that could improve the situation (although none can really solve the problem of the alleged gap forming between revenue and expense). My personal opinion is against UBB since I believe it is against innovation and will make Canada less competitive. Charging per-byte rates will become far too expensive for many people to use the Internet in the same way as people in other countries (or those people with lots of extra money). It will result in contributing to the gap between have and have-nots where poorer people in the country are excluded from the same access to content and opportunity that others have. I try not to focus on arguing for UBB in this article because there are several existing articles which argue this quite well. ([Financial Post The Globe and Mail](https://www.theglobeandmail.com/news/technology/tech-news/ubb-internet/the-public-is-right-to-be-cynical-of-internet-usage-regulators/article1898151/)).

In May of 2010, I attended an IEEE conference called the International Communications Conference (ICC 2010) in Capetown, South Africa. This conference is considered the one of the top conferences by the IEEE communications society and is attended by many experts in the communications field. I found one of the keynote speeches by Dr. Steven D. Gray, Head & Vice President Corporate Research, Huawei Technologies to be especially relevant to the current debate in Canada over usage based billing (UBB). The keynote is available free online from the [IEEE ComSoc](https://www.comsoc.org/webcasts/view/accelerating-growth-future-services-media-centric-networks) (please note, the keynote I am referencing here starts at about 51 minutes in the presentation).

While the key point about profitability deals with wireless carriers, it may also be relevant to wired network providers. Especially in Canada for the following reasons:

1. The vast, sparse country which must be connected
2. The constant upgrades to equipment required
3. The limited capabilities of backbone and infrastructure networks & increasing speeds of user connections
4. Steady growth in traffic from users

## 1. The vast, sparse country which must be connected

Compared to other countries in the world, Canada is very sparse. According to a [Wikipedia article surveying population density around the world](https://en.wikipedia.org/wiki/List_of_countries_and_dependencies_by_population_density), Canada places 228 in the world. According to [this figure (from Gizmodo)](http://cache.gawker.com/assets/images/gizmodo/2009/10/raw.jpg), Canada ranks 8th in the world for average broadband speeds. Compared with the USA, which ranks 15th (and has a population density almost 10 times higher than Canada), we have less than twice the average speed for roughly the double the cost. Considering our low density and similar geographic size, one would expect we would pay much more.

![Map of the Internet](/assets/img/connection-map.png)
[Map of the Internet](http://www.opte.org/maps/)

## 2. The constant upgrades to equipment required

The communications industry is constantly evolving as engineers and scientists figure out how to make faster connections or find new ways to communicate (for example, 10 mbps ethernet to 100 mbps to gigabit and beyond), (example 2: Ethernet to fibre optic, or even next generation wireless networks). Every time something new comes out or improvements are made, the companies must put money into making it work with what they already have, deploy new equipment and so forth. Often because the networks are becoming more complex it requires hiring more people to manage them (which may eventually be solved by autonomous networking, but that's a different story).

Another example of expense to keep up to date in Canada is Bell Canada. Many people with Bell Internet also have wireless access points provided by Bell. Unfortunately, these access points default to (or in some cases only support) WEP encryption. This encryption is not secure at all and can be broken by your average teenager with instructions off [youtube](https://www.youtube.com/watch?v=3seUWVK_Tb0). However, it could be argued in hindsight, this type of expense is the company's fault.

Furthermore, to increase capacity in the network, it is not as simple as just adding another link between an under-supplied area. Consider a small town where a few people make use of the majority of the connection cause poor performance for the others in the town. This town may be a candidate for increased capacity, but by adding another connection the company does not stand to gain any increase in subscribers and thus may stand to lose money. Now expand this example to larger cities where higher and higher proportions of the population are using more and more of their connection to the point where almost everyone is straining the infrastructure. There are solutions to this type of problem though. Some companies in the US and other places make use of traffic shaping (which while unpopular is a cheaper alternative to the user). Perhaps two types of plans could be put in place, one with traffic shaping and unlimited usage, or one with no traffic shaping and limited usage.

## 3. The limited capabilities of backbone and infrastructure networks & increasing speeds of user connections

One of the problems with the increased cost to the providers is the increased speed of user connections. Allowing users to have faster connections means they can request more information at once. Imagine many people flushing their toilets at the same time where the pipe at the road isn't big enough to handle all of the water at once. This is what has been allowed to happen. The problem is further compounded because it is not as simple as water simply flowing through pipes. At each junction, decisions must be made on the direction of the flow. If too much traffic arrives at once junction, the time it takes to make a decision is slower than the rate new traffic is appearing and big problems happen.

## 4. Steady growth in traffic from users

As can be seen in some of the figures in the ICC keynote, traffic from users is growing exponentially. Unfortunately, the service providers' revenue is usually growing linearly, so there is eventually a point where it is not profitable for the companies to provide Internet service (assuming the cost to provide exponential traffic grows exponentially). This is the most compelling point for usage based-billing. Unless we can somehow find a way to reduce the growth of data, the only way to retain profitable ISPs is to increase the revenue.

![Internet Growth](/assets/img/cisco.jpg)

Internet growth [Cisco](https://www.cisco.com/c/en/us/solutions/service-provider/visual-networking-index-vni/index.html)

## 5. Conflicting interests between user service and shareholders profits & the disconnect between what you get and what is advertised

This is another important problem with ISPs. Perhaps this is the reason that the infrastructure managed to get into its current state (where it cannot handle everyone making full use of their advertised connections). In order to gain a competitive advantage over competing ISPs, each company often advertises their maximum potential connection speeds. There is never any mention of the network past the point where your house connects to the ISP. Sure if no one else in your neighbourhood is using the connection you might get close to the advertised speed, but as many people know, you often don't get close. From the shareholder point of view, you want the company to spend as little as possible on infrastructure. Perhaps it may be necessary the enforce a rule that companies must be able to provide the full advertised speed, regardless of how many other people are on the network. Of course, this would make the Internet much more expensive.

## 6. Potential Solutions instead of UBB

There are many promising technical solutions to some of the problems that are causing the gap between cost and revenue in Internet technologies, however many of them may not make enough of an impact, or are still too premature to cause a difference yet. So in the meantime something must be done.

In the case of providing video services or other multimedia streaming services such as Netflix, LastFM etc. technologies such as multi-casting may be used to deliver common data to a group of users with one packet (in the unicast model, one packet is sent over and over again for every user, even if they are watching the same content).

To reduce the cost of human maintenance and oversight over the networks, applying autonomic computing techniques to networks so that they can self-manage, self-protect etc. may be beneficial.

Exploiting peer-to-peer, caching and other technologies that reduce communications over the large distances on the Internet may help reduce the cost of delivering traffic on the Internet.

Providing tiered Internet service, where users pay for different service levels may be another model altogether that allows companies to remain competitive without usage based billing. Make traffic that causes high strain on the networks (such as streaming video, real-time traffic etc.) more expensive than traffic that is delay tolerant and has lower requirements (web, email etc.)

Perhaps the government itself should take a more active role in providing Internet infrastructure in Canada if the Internet is seen as another piece of infrastructure like roads, bridges and electricity.

## Concluding remarks

Of course, there are many assumptions to the case for UBB. The assumption that the cost for service providers is growing exponentially is the biggest and most important. Wired network access doesn't have the same problems wireless has (the broadcast, limited bandwidth medium, interference etc.) so the comparisons may not actually be valid. The case becomes complicated by the fact that many ISPs are also in the business of providing wireless service, media services (tv, radio, newspaper etc.) that causes conflicts of interest.

The trouble I have with charging the heaviest users is that everyone seems to be trending towards using more and more traffic. In my own experience with large ISPs I feel like I'm always getting less for more money (usage caps have been getting lower and lower, yet the bill keeps going up). The advertised speeds of the networks are going up, but the capacity seems to be falling since we can get more faster, but less overall.
