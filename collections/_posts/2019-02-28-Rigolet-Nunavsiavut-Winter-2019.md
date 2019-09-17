---
layout: default
title: ""
published: true
featured-image: ""
featured-alt: ""
categories: []
tags: []
---

{% include featured-image.html %}

This past week or so, I’ve been working out of Rigolet, Nunasiavut with my RightMesh colleague, Frazer Seymour, and MITACs researchers, Prof. Dan Gillis and graduate student Nic Durish.

We had a few goals with our trip:
1. Develop some tools and deploy some devices in the community that will help us better understand device density, network traffic patterns, and user mobility.
2. Engage with the community by holding an open house and visiting the local school.
3. Attend meetings in Goose Bay about expanding efforts to other Nunatsiavut communities.
4. Conduct field tests of RightMesh with Autonomous Connectivity in Rigolet.

**Engaging with the Community**
As I’ve learned quite often visiting this remote community, plans don’t always go as expected. Immediately after arriving, Dan got quite sick, and the rest of us also got sick to varying degrees. We had to cancel our open house to prevent spreading the illness to the rest of the town. We also had to cancel the meetings in Goose Bay due to a combination of people travelling, illness, and us being stuck in Rigolet longer than expected due to a winter storm.

Luckily, a couple days later Nic, Frazer and I were all feeling well enough to visit the school where Nic and Frazer gave a couple of classes. Frazer taught about the basics of how the Internet works, how pages are served using [Flask](https://palletsprojects.com/p/flask/) (and how to quickly build some basic web apps). Nic gave a hands on demo of the [Microbit](https://microbit.org/), explained the difference between micro-controllers and micro-computers, and showed the students how to connect peripherals to the device and program them. They also gave a short concert on guitar and piano (to show that computer scientists can do more than just sit at a computer all day). The entire school came out to watch. It was great to see some of the graduate students I am mentoring passing on their expertise to other students!

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Micro bits were a great new project for the students! They really enjoyed the hands-on experience! Thanks to <a href="https://twitter.com/Durishn?ref_src=twsrc%5Etfw">@Durishn</a> <a href="https://twitter.com/FrazerSeymour?ref_src=twsrc%5Etfw">@FrazerSeymour</a> <a href="https://twitter.com/compscidr?ref_src=twsrc%5Etfw">@compscidr</a> <a href="https://twitter.com/DrDanielGillis?ref_src=twsrc%5Etfw">@DrDanielGillis</a> <a href="https://twitter.com/UofGComputing?ref_src=twsrc%5Etfw">@UofGComputing</a> <a href="https://twitter.com/This_is_Left?ref_src=twsrc%5Etfw">@This_is_Left</a> <a href="https://twitter.com/Right_Mesh?ref_src=twsrc%5Etfw">@Right_Mesh</a> ! <a href="https://twitter.com/hashtag/comebackagain?src=hash&amp;ref_src=twsrc%5Etfw">#comebackagain</a> <a href="https://twitter.com/hashtag/interactive?src=hash&amp;ref_src=twsrc%5Etfw">#interactive</a> <a href="https://twitter.com/hashtag/technology?src=hash&amp;ref_src=twsrc%5Etfw">#technology</a> <a href="https://twitter.com/hashtag/microbit?src=hash&amp;ref_src=twsrc%5Etfw">#microbit</a> <a href="https://twitter.com/NLAEagles?ref_src=twsrc%5Etfw">@NLAEagles</a> <a href="https://twitter.com/NLESDCA?ref_src=twsrc%5Etfw">@NLESDCA</a> <a href="https://t.co/r3OeCoUsoZ">pic.twitter.com/r3OeCoUsoZ</a></p>&mdash; Ms. Morris (@K1class_NLA) <a href="https://twitter.com/K1class_NLA/status/1100428642755231750?ref_src=twsrc%5Etfw">February 26, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Our grand finale: Country Roads performed by the whole school of NLA and our talented guests <a href="https://twitter.com/Durishn?ref_src=twsrc%5Etfw">@Durishn</a> <a href="https://twitter.com/FrazerSeymour?ref_src=twsrc%5Etfw">@FrazerSeymour</a> <a href="https://twitter.com/compscidr?ref_src=twsrc%5Etfw">@compscidr</a> <a href="https://twitter.com/DrDanielGillis?ref_src=twsrc%5Etfw">@DrDanielGillis</a> <a href="https://twitter.com/UofGComputing?ref_src=twsrc%5Etfw">@UofGComputing</a> <a href="https://twitter.com/This_is_Left?ref_src=twsrc%5Etfw">@This_is_Left</a> <a href="https://twitter.com/Right_Mesh?ref_src=twsrc%5Etfw">@Right_Mesh</a> <a href="https://twitter.com/NLAEagles?ref_src=twsrc%5Etfw">@NLAEagles</a> <a href="https://twitter.com/NLESDCA?ref_src=twsrc%5Etfw">@NLESDCA</a> <a href="https://twitter.com/hashtag/music?src=hash&amp;ref_src=twsrc%5Etfw">#music</a> <a href="https://twitter.com/hashtag/countryroads?src=hash&amp;ref_src=twsrc%5Etfw">#countryroads</a> <a href="https://twitter.com/hashtag/singalong?src=hash&amp;ref_src=twsrc%5Etfw">#singalong</a> <a href="https://twitter.com/hashtag/jamsession?src=hash&amp;ref_src=twsrc%5Etfw">#jamsession</a> <a href="https://t.co/EnOvd1mVll">pic.twitter.com/EnOvd1mVll</a></p>&mdash; Ms. Morris (@K1class_NLA) <a href="https://twitter.com/K1class_NLA/status/1100434754325921792?ref_src=twsrc%5Etfw">February 26, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**Autonomous Integration Progress**
The field testing of RightMesh was dependent upon me getting the last of the Autonomous work integrated into the codebase. Unfortunately, this has fallen a little behind schedule and while I worked on it most days while I was up here for around 10–15 hours per day, I didn’t get a chance to complete it while we were still here, so it will need a subsequent visit. Despite the incredibly slow, unreliable, and frustrating Internet access, the quiet isolation of being up here has accelerated the development quite a lot, and I expect to still have everything complete in time for our end of March deadline.

**Tool and Device Deployment**
Nic has been working with me to develop some tools for Raspberry Pi’s which perform measurement of throughput and round-trip time so that we can get a full-picture of the constraints of the infrastructure which does exist in Rigolet. We have distributed these around the community for people to plug into their home routers to perform measurements. We are waiting for research approval from Nunatsiavut government, but after this is obtained we will publish our findings. [This approval is so that Nunatsiavut knows what research is occurring in their own territory and can verify it is in their best interest](https://www.itk.ca/wp-content/uploads/2018/03/National-Inuit-Strategy-on-Research.pdf).

During one of the weekends up here, as a break from working on RightMesh codebase, I did some development on a library I’ve been working on as open source in my spare time for quite a while. This library performs Wi-Fi and Bluetooth scans, collects GPS, battery, and connectivity data.

Collecting this data will allow us to understand mobility patterns, device density, the windows of time devices will have to make a connection with each other as they are passing by each other, the impact of Wi-Fi and Bluetooth scanning on battery life, and much more. Ultimately, it will enable us to build a much better mobile mesh network.

While there are other apps out which collect similar data, this may be one of the first which aims to be open source, and open data. The library source is available on Github and can be built into any Android app:

[https://github.com/compscidr/awm-lib](https://github.com/compscidr/awm-lib)

A sample app I’ve built which uses the library is available on [Google play](https://play.google.com/store/apps/details?id=io.rightmesh.awm_lib_example) and also has source available on Github:

[https://github.com/compscidr/awm-lib-example](https://github.com/compscidr/awm-lib-example)

Finally, the server which acts as a data endpoint and map is also open source:

[https://github.com/compscidr/awm-lib-server](https://github.com/compscidr/awm-lib-server)

![](/assets/img/awm-lib.png)
![](/assets/img/awm-lib1.png)

The interesting part about working on these tools while in the community is that issues came up which unexpected due to a) not having connectivity while collecting the data which required developing a data cache and having the cache automatically upload the results when connectivity was available, and b) challenges with batch uploads of the data with extremely slow connections which may not have been encountered had development occurred in Vancouver. We also had the unusual challenge that the phones would occasionally shut off because it was so cold (so things had to be committed to persistent storage often or lost).


Currently, we are on day two of waiting anxiously to see if the winter storms will subside enough to allow us to return so we can get our connecting flights. Frazer will head to Vancouver, and I’ll be heading to Guelph for some continued work with Dan and Nic.

![](/assets/img/windy.png)
