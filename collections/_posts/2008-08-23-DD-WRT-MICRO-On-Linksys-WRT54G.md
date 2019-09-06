---
layout: default
title: "DD-Wrt Micro on Linksys WRT54Gv6"
published: true
featured-image: "wrt54gv6.jpg"
featured-alt: "WRT54gv6"
categories: [Linux, Wireless Networks]
tags: [DD-WRT54G, Linux, Router, Wireless, AP]
---
Today on a whim I decided to pickup a Linksys WRT54Gv6 wireless router from Staples. It was on sale for $20 off and looked like the last one there. I've been looking at the DD-Wrt software for some time since I have an old junky wireless router buried in my closet at home. Unfortunately the old router I have cannot run DD-Wrt so when I saw this one on sale I thought I'd give the WRT54G a try.

{% include featured-image.html %}

Unfortunately, I didn't realize I bought the crippled version of the good WRT54G since it now has only 2mb of flash memory rather than the 4mb the earlier versions had. Apparently the best version to get now is the WRT54GL because it contains the most flash memory and is meant for Linux. The version I bought has a proprietary O/S installed on it and I was slightly worried I wouldn't be able to get linux on it, but after following some good instructions at the Scorpiontek website I managed to get it working really easily.

I've only have the new firmware on the router for a few hours but I already like it alot more than the stuff that it shipped with. Even with the micro version there are far more options to play around with. The most useful so far for me is the client bridge mode. At my girlfriends house, her Access Point (AP) is located down in the basement and we use the net mostly in her room on the second floor. Until now we had terrible connections up there but in client bridge mode we can make use of the more powerful radio on the AP and plug into it with wires. Seems to be working much more smoothly now than when we had several weaker clients connected in the same room. Perhaps ill try and make some graphs and show if there is an actual difference if I get some time.

Eventually I would like to try to play around with the DD-Wrt source and try to implement some of my wireless mesh ideas in a working device so I'll see if i can mange with what I've got here. Additionally, I have a cool looking LCD panel that an electronics friend gave me so I'll see if I can get some help hooking that into the router to display some useful information right on the router. I'll post any progress I make or pictures of mods on the blog here.
