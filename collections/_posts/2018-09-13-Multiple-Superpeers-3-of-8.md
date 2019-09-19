---
layout: default
title: "Multiple Superpeers Post 3/8: Remote Peer Bootstrapping, Superpeer Discovery, Ranking, Selection"
published: true
featured-image: "roadmap3.png"
featured-alt: ""
categories: [Technical Roadmap, Wireless Networks]
tags: [RightMesh, Decentralization, Bootstrapping]
---

This is the third in a series of RightMesh Roadmap posts. Please refer to the [initial post for naming conventions and assumptions](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-implementation-plan-and-progress-e637be9d53fb). Links to other posts in the series are listed at the end of this article.

There are a few critical problems that must be solved here — bootstrapping the phones to the RightMesh Superpeer network, discovering other potential Superpeers, and selecting the appropriate Superpeer(s) to make a payment channel with. Note that this is separate from actual data routing. When bootstrapping, like bootstrapping between Superpeers, we desire that this is decentralised as possible. When selecting Superpeers, the Buyers in particular wish to minimise their costs to establish payment channels with Superpeers, and likely wish to not lock all of their funds in one particular Superpeer. Since Sellers are not paying to establish their channels, they don’t optimise the same way — and will take whatever Superpeer offers the best performance and lowest fees.

Keep in mind, the Buyers are paying at least for their initial channel to the Superpeer. Some Superpeers (like our reference implementation), will refund this cost, provided the Buyer has moved the appropriate number of tokens through the channel to the Superpeer), thus rewarding the Buyers who spend enough money to make the Superpeer payment channel calculations profitable. This discourages Buyers from frequently selecting different Superpeers too often as conditions change (prices, performance, etc). The Buyer algorithm must weigh the cost of switching (ie: the lost Ethereum for the channel creation, against the potentially lower fees, or better performance of another Superpeer).

Data Sellers may wish to switch for similar performance and fee reasons as well. We are making the assumption that the Superpeer funds the creation of the payment channel between the Superpeer and the Seller, and loads it with at minimum, the amount of tokens that it determines will earn profit (considering the cost to reload or reopen the channel at some later point). More details and examples with numbers will be provided in a subsequent post detailing specifically the Payment Channel Balancing Algorithm.

Similar to the Superpeer to Superpeer bootstrapping, the phones (both Buyers and Sellers) themselves need to select a Superpeer as the initial connection point into the Rightmesh network. Currently, the phones by default select superpeer1.rightmesh.io. We intend to implement a system similar to the Superpeer to Superpeer discovery, whereby we run several main RightMesh superpeers, which the phone randomly selects for Bootstrapping.

![](/assets/img/bootstrap.jpeg)

Whichever RightMesh Superpeer responds to the request should have a global view of all other community Superpeers, so it will respond with a list of the all of the Superpeers it knows about.

This would occur during the initial INFO ACK packet that is sent from the first bootstrapping RightMesh Superpeer back to the Remote Peer.

![](/assets/img/bootstrap1.jpeg)

While RightMesh Remote Peers can have an InternetLink connection to many Superpeers, it should limit the number of Superpeers which actually have a payment channel associated with them. (In fact in the short term, we should probably only support a single SP connection at once initially to keep complexity down — just keep it in mind that later on we may have multiple connections at once to multiple SPs concurrently).
In our meetings it was initially proposed that the phones send their weighting of the utility function to the bootstrap Superpeer and the Superpeer would respond with the appropriate Superpeer to connect with, however, we have changed out mind about this for two reasons 1) it puts too much trust in us to determine the appropriate Superpeer. 2) It relies only on information which is measurable between Superpeers, it should be measured at least partially between the remote device and the Superpeers in question. 3) It doesn’t allow decentralised bootstrapping in subsequent re-connections (aside from just connecting to the same Superpeer as last time).

Assuming we get a list of all of the Superpeers back from the bootstrap Superpeer, the remote device must verify the signature of the list, store the signed list, rank the Superpeers based on which Superpeer the Seller(s) are connected to, select the highest ranked Superpeer, propose a channel request to the Superpeer, wait for acceptance, update the routing costs, and periodically re-evaluate.

For the Buyers there are intervals when re-evaluation is natural: 1) when a payment channel has run out of funds and a top-up is needed. 2) when a Superpeer has closed a payment channel to recoup the funds. 3) When the performance of another (Superpeer, Seller pair) outweighs the preference for minimising price.
For the Sellers, it makes sense to re-evaluate when 1) The Superpeer side of the channel is empty and we have not received a request to hold the channel open — the Superpeer may not be willing to commit to continue to fund our channel. 2) The performance of the Superpeer is too low. 3) Many of our Buyers are attached to another Superpeer, if we compute we could reduce the proportion of costs lost to inter-Superpeer transfers it may make sense to switch.

**Tasks:**
* Modify our current RightMesh Superpeer so it no longer creates a payment channel immediately upon the PeerEvent firing — it should only do this when a Superpeer has been selected.
* Work with the Faucet team so that instead, if this is the first time ever this peer has joined, fund the Remote Peer.
* For PoC of the funding, allow manual override of the Superpeer (avoid bootstrap) via a public function in the API, replace this with bootstrap test below.
* Modify the INFO ACK to contain a list of all possible Superpeers, plus various metrics that can be delivered from the Superpeer side to aide the in decision (for now leave this vague and only support a view key metrics: SP->Buyer fee, SP->SP fee, minimum number of tokens in a channel, number of connected data Sellers, number of connected data buyers, average number of connected Buyers per data Seller, uptime, throughput on first boot). Make this extensible so we can add more metrics later. These can mostly be computed by the RightMesh Superpeers by looking at the routing table (with a slight modification so the RightMesh Superpeer can compute the uptime of other community Superpeers).
* For PoC, pick random Superpeer from list (to test bootstrap), select it, replace this with actual selection below.
* Add a component to the Community Superpeers which performs a throughput test with a server we operate for this purpose and have it report the result back to the RightMesh Superpeer so we know the bandwidth available to split amount all the data Sellers (Remote Peers can then divide this amount by the total connected data Remote Peers to determine how much bandwidth they might be able to expect).
* Make this INFO packet response from the RightMesh Superpeers signed so it cannot be modified. The raw signed data of the Superpeers should be stored in a file and verified upon future reload by the phones that it is has not been tampered with since the last bootstrap.
* Make the initial selection of the Superpeer based upon a utility function which balances cost and performance: u(SP) = weight * performance(SP) + (1-weight) * cost(SP) with a starting weight of 0.5. And performance function of performance(SP) = 1/#metrics*metric1 + 1/#metrics*metric2 + … + 1/#metrics*metricn. The cost function is not only in terms of the cost to use the SP but also depends on which data Seller the Buyer is intended to use. If a cost of a SP + Seller together is higher than the buyer threshold, the u(SP) price should be zero.
* Implement a new request type (or modify the existing GET_ALL packet) so that the Remote Peer requests for a Buyer channel to be open with the proposed funds. If the funds are too low, the Superpeer should reject creating the channel. If a Superpeer rejects creating the channel, the Buyer should remove this Superpeer from their candidate list and proceed to next highest ranked Superpeer. Note: Superpeers may also reject a connection if they have no available tokens or eth for whatever reason.
* Update the way route costs are computed in the routing table so that they include the fee price of using the selected Superpeer as well (or alternatively compute the route costs for using the same Superpeer as the Seller vs otherwise).
* Update the control packets so that the selected Superpeer of devices is known (this will help with computing the costs).
* Implement a request to notify the Superpeer that this Buyer intends to top-up the payment channel (to discourage them from closing the channel and reducing costs). This should trigger the Superpeer to refund the channel reloading cost, otherwise a recalculation should occur which takes into account this extra cost, and perhaps a switch should be made.
* Implement a modification to the utility function so that Superpeers which have not refunded the cost of channel creation when the channel closes get ranked lower (or maybe even removed from the list).
* For testing, implement a basic slider UI which has cost, performance and middle. In practice for the competition we may assign these randomly, or make people stake to unlock this feature. Before the UI is done, we can just hardcode values in for peers, or use a manual override function for testing.
Over time (at minimal priority, add more metrics, some of which can be collected from the end device side).

With the first two posts in this series, we outlined what needs to be done to bootstrap the Superpeers, and the Remote Peers to the Superpeer network, as well as some of the request and responses that need to be created in order to facilitate channel creation. At this point — the Superpeers know about each other, and the Remote Peers know about one particular Superpeer and can make a request to establish a payment channel (A future post will get into the details of how a Superpeer will decide to open the channel or not). We still need protocols which allow the Superpeers to exchange which Remote Peers each knows about, and then communicate that information to the Remote Peers as well. The following post will cover the Superpeer Peer Exchange Protocol (PEX).

This post originally appeared on Medium: [Multiple Superpeers Post 3/8: Remote Peer Bootstrapping, Superpeer Discovery, Ranking, Selection](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-2-remote-peer-bootstrapping-superpeer-discovery-bbd8717ff464)
