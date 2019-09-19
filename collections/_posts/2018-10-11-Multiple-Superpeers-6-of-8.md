---
layout: default
title: "Multiple Superpeers Post 6/8: Superpeer Caching"
published: true
featured-image: "roadmap6.png"
featured-alt: ""
categories: [Technical Roadmap, Wireless Networks]
tags: [RightMesh, Superpeer, Caching, Mesh]
---

When we send data from the mesh to the Internet (or from the Internet to the mesh), we only want to send it once so if data is lost between the Superpeer and the receiver in another mesh, the original source doesn’t need to pay for Internet data multiple times. How can we cache the data at the Superpeer so this is possible? Do we cache it at the source or the destination Superpeer? How do we pay for it? How do we avoid the same problem on the receiving side? Can we introduce caching at the data Seller to prevent the InternetLink being used over and over again if there is a problem delivering it locally?

Consider the following network topology. The links between S1 and RSP1 and S2 and RSP2 are the Internet Links. These are the limited resource we are trying to make the best use of. We want to avoid re-transmissions across these links wherever possible.

![](/assets/img/topology.jpeg)

Consider a source of B2 and a destination of B5. Along the transmission suppose the link between S2 and B3 is not very reliable and goes down for long periods of time. In our current RightMesh implementation, while we support a limited number of local one-hop re-transmissions and also end-to-end reliability, it may be that the link is so unreliable the one-hop re-transmission gives up, and the data send is left to fall back to starting from the source. We want to avoid this case wherever possible. We can take advantage of the Superpeers being reliable, having fast stable connections, and more resources (memory, storage) than the Remote Peers.

![](/assets/img/topology1.jpeg)

During the session, we agreed that the Superpeer on the sending side would be responsible for caching the data and would be responsible for delivery to the receiver. The idea is to minimize the transmissions across the InternetLink in the case that the local mesh breaks down and becomes unreliable.

To prevent re-transmissions across the S1 to RSP1 link we can cache the data at the RSP1 Superpeer. However, if there is a loss in the destination remote mesh, there will still be retransmissions across the RSP2 to S2 link.

![](/assets/img/topology2.jpeg)

Further, we want to minimize the number of re-transmissions on the InternetLink on the receiver side. We can do this by also caching the data at the Seller and then making the Seller responsible for the delivery to the destination.

![](/assets/img/topology3.jpeg)

There are a few modifications needed to our data sending process now to support this:

1. Our data flow end-to-end ACKs must now come from the caching nodes. The data packets themselves must keep the destination address, and also have a caching address.
2. The data packets now have a new type of delivery state, the first being “delivered to the cache on the Internet” and “delivered to the final destination”. This requires a couple of new event types being fired.
3. We need a new type of end-to-end ACK which is delivered from the destination to the Seller cache. This causes the Seller cache to remove the cache, and then send the same end-to-end ACK to the Superpeer cache, causing it to remove the cache and then send the same end-to-end ACK back to the source. At this point, the source can consider the data delivered to the destination.

![](/assets/img/topology4.jpeg)

One important case we need to consider is if the destination changes its Seller, either by switching due to prices or by physical movement. In this case, the Superpeers will eventually learn of the change via INFO packets, and at this point, they will send a DEL CACHE packet to clean up at the old Seller. It will then initiate a transfer from the Superpeer cache to the new Seller.

![](/assets/img/topology5.jpeg)

We also discussed what happens if a caching Superpeer goes down. For the launch, we will operate on the assumption that the Superpeers will not go down (unrealistic we know, but we can solve this problem with redundant mirrors later).

Currently, in our protocol, we have an end-to-end ACK which is used for signaling to the sliding window protocol that packets have been received at the destination. We also use it to signal to applications that the data has been received. If a caching Superpeer goes down, there are a few options — we can notify the application that the last transmission failed because of the Superpeer, and they can retry with whatever new Superpeer gets selected, they can wait for a timeout type of event on the reception, etc.

In order to support caching, the end-to-end ACK is no longer coming from the destination. It is coming from the caching Superpeer. The caching Superpeer then issues a new send to the final destination and it then receives the end-to-end ACK. It must now construct a new type of end-to-end ACK to signal the delivery of the entire stream of data so end-user applications are aware that the datastream was successfully delivered (otherwise they will only get notification of this when it has reached the cloud and not the final destination).

Further, to avoid retransmits over the receiver side Internet connection, the data Seller itself must cache the stream as well. This way if there is local mesh unreliability, the data Seller can try again for delivery. So, the Caching Superpeer must issue a send to the appropriate Seller. It would receive end-to-end ACKs for its sliding window protocol the whole time. The caching Superpeer should not remove the data stream yet, however. The data Seller should repeat the same process to the receiver. If the receiver successfully receives the data and sends an end-to-end ACK for the last packet, then the data Seller should create this new type of end-to-end ACK and send it back to the caching Superpeer, and also remove the cached data. When the Superpeer receives this packet, it should remove the cached data, and send this new end-to-end ACK back to the original sender, at which point an event should be generated notifying the app that sent the data that it has been sent and delivered successfully. Note that progress bars based on the sequences being received are probably not worthwhile to do in this case because the overhead in these extra end-to-end packets is not worth it — progress bars can still be based on the old-style delivery to the cloud.

**Tasks:**
* Ensure the receiver side can make known to the Superpeer which Seller it intends to use (so the Superpeer delivering the data does not select a Seller which is out of the price range or performance requirements of the user). Consider implementing a new request type for the Buyer to notify the Superpeer it would like to switch its Seller (in case of a price change or performance degradation).
* Implement caching at the sending Superpeer with a finite data storage space (say 1000mb). When the space has emptied, come up with a strategy for removing items…oldest, biggest, whatever. Change the sendDataReliable function so it sends to the Superpeer if the peer is not local. Have the Superpeer send the ACKs. Have the Superpeer issue a subsequent sendDataReliable to the destination remote peer. Clean up the cache upon reception of end-to-end ACK.
* Implement caching at the receiving data Seller with fixed storage size (say 100mb). Adjust this process so it now supports the data Seller also caching. May have to come up with a process to send the cache from the Superpeer into smaller pieces (since the phone caches will likely be much smaller, but only send a final removal event back to the original sender when all of the cache has been delivered to the receiver).
* Handle the case where the receiver switches Sellers. Have the sending Superpeer notify the old Seller to remove the cached item, send the data again to the new Seller.
* Add the new end-to-end ACK packet so end applications can know if the reception has happened.

## What we will implement in the following 4–6 months afterward?
During the contest, we will attempt to address more fully the problem of Superpeers going down resulting in the cache being lost temporarily and extremely long delivery times. We have had proposals of groups of Superpeers that work together to deliver things — using DHT or other techniques. This will require more thought and planning to execute on.

This post originally appeared on Medium: [Multiple Superpeers Post 6/8: Superpeer Caching](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-5-superpeer-caching-a539080b20f6)
