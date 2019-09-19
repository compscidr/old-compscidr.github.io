---
layout: default
title: "Multiple Superpeers Post 4/8: Superpeer Peer Exchange Protocol"
published: true
featured-image: "roadmap4.png"
featured-alt: ""
categories: [Technical Roadmap, Wireless Networks]
tags: [Mesh Networks, Peer Exchange, Mobile Mesh]
---

The Superpeer Peer Exchange Protocol is how the Superpeers exchange all the peer information. In RightMesh currently, the INFO packet is used to send Remote Peers from remote meshes to local meshes and from local meshes to the Superpeer which sellers are attached to.

When we have multiple Superpeers; however, we need something which can exchange peers between the Superpeers, otherwise we’ll have splintered meshes. There are scalable ways in which to do this, such as with Distributed Hash Tables (DHT), but for the sake getting to launch faster, we are going to continue using our HELLO packets between Superpeers initially. This is the easiest way to get running without extensive modifications.

The following series of diagrams illustrates how the INFO packets from the local meshes to the Superpeers interacts with the HELLO packets between the Superpeers to enable discovery of Remote Peers across multiple meshes through multiple Superpeers.

Initially, let’s assume we start with just a single RightMesh Superpeer running.

![](/assets/img/peerexchange.jpeg)

When the first Remote Peer joins the network using the Bootstrapping approach we have [previously described in the last post](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-2-remote-peer-bootstrapping-superpeer-discovery-bbd8717ff464), it will randomly select a RightMesh Superpeer to connect with. It sends an INFO packet to this Superpeer.

![](/assets/img/peerexchange1.jpeg)

The Superpeer adds the Remote Peer to its routing table and sends back an INFO acknowledgment. Since there are no other devices attached to the Superpeer, the packet is empty and the Remote Peer adds only the Superpeer to its table. At this point, the Remote Peer (S1) would consider forming a payment channel with the Superpeer (RSP1).

![](/assets/img/peerexchange2.jpeg)

Next, another Remote Peer joins the network as a buyer (B1), via the previous Remote Peer acting as a seller (S1). It sends a HELLO packet in the local mesh which causes S1 to add an entry to its local table.

![](/assets/img/peerexchange3.jpeg)

S1 sends back an acknowledgment for the HELLO containing all the peers it knows about (RSP1) at which point B1 adds both S1 and RSP1 to its local table. At this point, B1 would consider making a payment channel with RSP1.

![](/assets/img/peerexchange4.jpeg)

Just to make things clear, let’s assume another buyer joins, called B2. It goes through the same HELLO process as B1. Notice that the Superpeer RSP1 has not learned about B1 yet — it finds out about these new peers via periodic INFO packets sent from the Remote Peers connected directly to it (via S1).

![](/assets/img/peerexchange5.jpeg)

When the acknowledgment is sent from S1 back, it may be broadcast within the local 1-hop network so that both B1 and B2 can learn information at the same time.

![](/assets/img/peerexchange6.jpeg)

Let’s assume now that a second RightMesh Superpeer is joining the network (RSP2). It sends a HELLO packet to the other running Superpeers. At this point, RSP1 adds the new Superpeer to its local table. Let’s also assume that an INFO packet is being sent from S1 to RSP1 to update the RSP1 about the local Remote Peers.

![](/assets/img/peerexchange7.jpeg)

Assuming the HELLO packet is processed before in INFO packet at RSP1, the response from RSP1 to RSP2 will only contain the older information of S1 and RSP1. Afterwards, when RSP1 processes the INFO packet, it adds the new Remote Peers to its table. In the response, S1 learns about the new Superpeer, RSP2. At this point, S1 can now make a choice between which Superpeers to make a payment channel with.

![](/assets/img/peerexchange8.jpeg)

At some point in the future, some of the Remote Peers will send a HELLO packet to obtain updated topology information. Similarly, the RSPs will do the same. At this point, the missing information at some of the Remote Peers will propagate to the rest of the network.

![](/assets/img/peerexchange9.jpeg)

![](/assets/img/peerexchange10.jpeg)

The remaining figures illustrate how a Remote Peer more than one hop away from the Seller can propagate into the Superpeer Network, and how Remote Peers from different Superpeers may discover each other.

![](/assets/img/peerexchange11.jpeg)

![](/assets/img/peerexchange12.jpeg)

![](/assets/img/peerexchange13.jpeg)

One place our existing protocol will likely quickly break, though, is the control packets themselves — the HELLO, INFO, etc. are limited to 8,000 bytes currently, and eventually, with enough peers, they will not all fit in the packet. In this case, the control packets should be able to split and recombine and this will need to be done before launch.

Also, to reduce the load on the phones of having to remember all the devices at once — we will restrict it such that devices will only “see” the live online status of devices in their local mesh. The assumption is that a person will likely meet the person which he/she would like to talk to locally first, and then keep in touch with them later. To support this, a FIND request is being added (which would translate into an API call on the MeshManager) in order to support finding the state of a peer who is not within the local mesh at that particular moment in time. There are also ways to add farther away peers, if the two Remote Peers have not met before within a local mesh. One could send the MeshId of the peer via SMS or some other system at which point a FIND request could find them. More details on how this will work is provided in a subsequent post about the FIND packets and how we will scale the peer discovery mechanisms.

**Tasks**
* Consider implementing a second table which only keeps track of Superpeer MeshID + their IP address, plus corresponding display and debugging functions.
* Consider implementing basic CLI functions to query routes from the live running Superpeer, displaying tables, etc.
* Periodic HELLO requests between Superpeers (threads dedicated to communicating with other Superpeers).
* Modification of the prepareHello and processHello functions for the special case that the devices are Superpeers.
* Implement HELLO packets which can handle 1000’s of MeshIDs and appropriate other metadata (without increasing the packet size, should be able to split and rejoin packets to do so). Quantify how many bytes per MeshID is sent (including meta-data) so we can determine how many bytes will be required to communicate between all of the Superpeers at every interval.
* Modify the INFO ACK from the SP to the phones so that it no longer returns all possible MeshIDs, only the list of SPs (for bootstrapping and periodic re-evals of which SP to connect with)
* Modify the INFO ACK response on the phone side so that it doesn’t add all other SPs into the routing table — we only really need to add the one we select. Maintain this list of possible SPs somewhere else for reevaluation.

This post originally appeared on Medium: [Multiple Superpeers Post 4/8: Superpeer Peer Exchange Protocol](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-3-superpeer-peer-exchange-protocol-eed33ff29f6c)
