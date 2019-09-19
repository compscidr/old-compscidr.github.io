---
layout: default
title: "Multiple Superpeers Post 5/8: Scalable Remote Peer Discovery"
published: true
featured-image: "roadmap5.png"
featured-alt: ""
categories: [Technical Roadmap, Wireless Networks]
tags: [RightMesh, Peer Discovery, Superpeer]
---

As mentioned in the [previous post](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-3-superpeer-peer-exchange-protocol-eed33ff29f6c), maintaining the full state of the entire global RightMesh network on every phone will not scale — so instead, we are proposing to make use of the hierarchy that exists in our system design so devices only need to know who is in near proximity, and then have the ability to query for devices which are farther away. We do this through what we are calling the “FIND” request.

The “FIND” request is meant to be a way for RightMesh peers to find out about the state of peers in a different local mesh, without every peer needing a full and up-to-date routing table including the entire mesh. The idea is that peers will send a new “FIND” request to a Superpeer to query the state of a peer in the mesh.

This proposal is split into two parts — how and when a local peer/mesh sends a FIND request, and how the Superpeer responds to and continues to update those requests.

## Scalable Remote Peer Discovery Part 1: When to send a FIND request

### “Remembered” Peers Database
A simple and naive approach to implementing FIND requests would be to only do exactly that — implement a method to query the status of a peer not on the local mesh. However, doing only this will likely result in most applications maintaining a list of peers that the user has interacted with/has an interest in, then sending FIND requests for each of these peers (meshIM, eNuk, and likely other internal apps already do this). By maintaining the list in the library, we can save RightMesh developers the effort and streamline the discovery process for network performance benefits.

Which leads to a new requirement for the library: maintaining a list of peers of which we always want to know the status. These peers can be called “remembered” peers, i.e. their information is remembered even when they are no longer online. There are two proposals for how to handle remembering peers. The first is to handle it at the protocol layer, remembering every peer encountered locally. The benefits of this is that it makes the remembering process invisible to developers — they can count on the fact that if they have seen a peer once, they will always get the most up-to-date information on that peer going forward. Drawbacks to this approach are that we will have the overhead of looking for every peer a peer comes across offline, and/or added complexity of pruning the list.

The other approach is to handle remembering in the application layer, providing a method like *remember(…)* to complement *find(…)*, indicating that this peer is important to your application and that you always want updates on this peer’s status. This approach affords some network performance advantages, i.e. we only FIND peers that are relevant, and we can choose to only send FINDs for peers in the database if the app that wants them is running. An app can choose to approximate the behaviour of the protocol-layer approach by just remembering every peer they see (with the benefit that we only need to look for them if this app is running). It also makes it more apparent to developers that they don’t need to implement this behaviour themselves instead of not relying on repeated FINDs.

As the title of the next section indicates, the application-layer *remember(…)* solution is preferred. Disregarding how adding to the table is implemented, the actual database can be quite simple, just a list of MeshId: Integer pairs representing a MeshId to query for and the associated port. An additional String column could be used to record the package name of the application remembering the peer, for use in pruning the table in the future. Either a magic number for the port (e.g. -1) or a separate table might be used to indicate a peer was added as a contact in the RightMesh controller and should always be found with a FIND. The opening/closing of this database could happen at the MeshService level - where platform-specific details and the lifecycle are handled. An open generic instance could then be supplied to the *MeshInterfaceManager* - where queries and insertions will be handled.

### New MeshManager Methods
![](/assets/img/mim-methods.png)

### Sending FIND Requests
Actually sending FIND requests should be fairly trivial. A new RequestType needs to be added to the MeshDns protobuf object. The existing Peers entry can be used to hold the peers being requested (probably just a MeshId to start, though eventually, we could maybe send a port too, for further optimisation on the Superpeer side). A small method will need to be created in *RequestHelper* to craft the packet. Logic for handling a response is outlined below.

FIND packets will be sent when a user calls *MeshManager.find(…)*, and at the start of the library containing all the peers in the “remembered peers” database.

## Scalable Peer Discovery Part 2: How the Superpeer handles FIND requests

### Responding to the initial FIND
Responding to an initial FIND request is easy — see if any of the peers are online in the routing table, add them to an INFO packet, and fire it back to the Gateway Peer that forwarded the request. The information will propagate from the GP.

### Updates to peers that were requested with FIND
*However*, the life of a FIND request needs to be a little more complex than that. If we only send one INFO packet with the information, the network that receives the update will never know when a found peer goes offline, or comes online, in the future. Therefore, we propose that for each connected gateway peer, a Superpeer keeps track of a map and two lists, as described below.

The map would be of <*MeshId*>s to List<*MeshId*>s, and would track which MeshIds are of interest to which local Remote Peers of the given Seller. So for example, if local peer 0xaaa… called *MeshManager.find(0xbbb…, 12345)*, there would be a mapping of *0xaaa… -> […, 0xbbb…, …]*. If *0xaaa…* ever goes offline, its entry can be pruned from the map as it no longer needs peer updates.

One array would be of the *MeshIds* a Seller has received an update for. As long as the MeshId is in that array, the Superpeer needs to keep sending updates for that peer to the Seller. When a peer in that array goes offline, it can be removed from the array. These peers need to be tracked separately from the map above as we still need to receive disconnect events even if the Remote Peer that was originally interested in the peer goes offline. Possible names for this map are *trackedPeers, onlinePeers, subscribedPeers, …*

The final array is the union set of the array above and each unique MeshId in the lists of the map. i.e., every MeshId requested by a FIND request and every MeshId that an update has been sent for. This is the list of MeshIds that the Superpeer needs to let the Seller know about. Every time an INFO packet is sent to a Gateway Peer, the peers included are the intersection of this final array and the Superpeer’s global routing table.

The following figures illustrate how the FIND request would work. [Consider the same network topology from our previous post](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-3-superpeer-peer-exchange-protocol-eed33ff29f6c). It is slightly different because none of the Remote Peers know about Remote Peers from other local meshes.

![](/assets/img/discovery-table.jpeg)

Let’s suppose B2 wants to query if S2 is on the network (because perhaps they have previously encountered each other locally and added each other as friends in a mesh chatting app, and now they are in separate neighbourhoods too far away to communicate with each other over the mesh).

![](/assets/img/discovery-table1.jpeg)

B2 sends the FIND request through the S1 data seller to a Superpeer. The Superpeer has the global state of which Remote Peers are presently online and can look up in its table whether it is online or not.

![](/assets/img/discovery-table2.jpeg)

In this case, S2 is online, so RSP1 responds back with an acknowledgment containing the S2 peer. If the peer was not online, the acknowledgment would come back empty. It is also possible for the FIND request to contain a list of MeshIDs that the local mesh is interested in so bulk requests can be made to reduce overhead. A further optimisation would be to send the FIND requests within the INFO packets since these are sent periodically anyway.

Data transmission may need to be slightly altered in that currently we only send if the device is present and online — we should modify this so if the device is not online in the local mesh, we should transmit to the Superpeer

**Tasks**
* Add FIND request to protobuf file.
* Add prepareFind, processFIND.
* Add a mm.find() function (to the public API).
* Discuss — add peers found locally automatically to a persistent dB or depend on an app call to a new persistPeer() function or something? In any case one or the other has to be done. The idea is that RightMesh library on start would call find with this group of peers in the request.
* Possible farther out tasks: build some easy ways to exchange MeshIDs with farther away people (could be combined with sharing encryption keys as well), sms, barcodes, etc.

This post originally appeared on Medium: [Multiple Superpeers Post 5/8: Scalable Remote Peer Discovery](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-4-scalable-remote-peer-discovery-99db1d926540)
