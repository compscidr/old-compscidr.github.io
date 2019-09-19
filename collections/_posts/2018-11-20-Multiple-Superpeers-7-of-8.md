---
layout: default
title: "Multiple Superpeers Post 7/8: Automatic Faucet"
published: true
featured-image: "roadmap7.png"
featured-alt: ""
categories: [Technical Roadmap, Wireless Networks]
tags: [Blockchain, Microraiden, RightMesh, Faucet]
---
While the previous posts in the series have been focused mostly around protocol aspects to making Multiple Superpeers work, this post and the final post in the series are focused more around the tokens and Ethereum— specifically how they are provided to end-users, how they are managed by Superpeers, and how economics fits in with the costs of establishing, funding, re-balancing, and withdrawing from in relation to data moving through the RightMesh network.

For the purpose of the upcoming RightMesh [Soft Mainnet Launch Competition](https://medium.com/rightmesh/rightmesh-technical-roadmap-the-next-6-8-months-and-beyond-4c0867306ce9), we want to make sure that end-users (Buyers and data Sellers) never need to worry about topping up their Ethereum or RightMesh token balance to participate — we aren’t testing whether people will go through all this trouble at this stage. We do, however want to ensure people have sufficient tokens to conveniently test the RightMesh network, and drive enough traffic through the Superpeers and the protocol which will give us better resulting data and give the end-users who try RightMesh apps a better experience.

**The RightMesh Testnet Faucet**

People familiar with existing Ethereum testnets such as Rinkeby may know that faucets [currently exist](https://faucet.rinkeby.io/) in order for people to accumulate testnet ETH in order to test out their applications. So, the faucet will be setup so that it monitors all accounts on RightMesh and tops them up.

![Example of an authenticated Rinkeby faucet which requires users to use a social network to post the Eth address to send the testnet ETH](/assets/faucet.png)

To reduce friction for early end-users of RightMesh (ie: people using RightMesh apps), when a device joins the network and the Superpeers become aware of it via the bootstrapping process outlined in previous posts, the RightMesh operated Superpeers will communicate with the faucet so that the end-users have an appropriate amount of testnet tokens and ETH to function.

Disclaimer: The number of testnet ETH and RMESH provided in this post are for illustrative purposes and may be subject to change before the competition begins.

We also want Community Superpeers to have their initial balance setup the first time they run automatically. Suppose the competition only allows Community Superpeers to have 50,000 non-refillable tokens. Also suppose it also allows them ETH which is refillable. According to some of our tests with how much gas it takes to establish a payment channel, ETH should be able to support 1000 payment channel creations. If the balance ever falls below ETH we will issue an additional 1 ETH. The faucet will also contain some checks and protections to ensure that Community Superpeers do not just transfer their ETH away as an attack on our system. During the bootstrapping process for the Community Superpeers, when they connect to the RightMesh Superpeers, during the competition — the first time their MeshID is encountered, the RightMesh Superpeers will notify the faucet to transfer the appropriate amount of testnet ETH to the Community Superpeers.

The remote peers (Buyers and Sellers) will obtain, for example, 1000 tokens, which will automatically refill with another 1000 tokens once the balance is below 100. They will also receive 0.01 ETH which should be good for about 10 payment channel creations. It will be refilled with another 0.01 ETH when the balance falls below 0.025 ETH. The RightMesh library running on the end-user phones is set to select Superpeers based partially on the lowest price and partially on performance. The algorithm may elect to change Superpeers if the performance drops too low, or the price becomes too high which is why some amount of ETH is required for the phones.

![](/assets/img/faucet1.jpeg)

**Limiting Attack Vectors During the RightMesh Superpeer Competition**
One of the possible attacks on the competition is if the Community Superpeers have Buyer or Seller accounts transfer them the funds. However, it won’t affect the competition because we will only count the incoming and outgoing payment channel balances (rather than just directly looking at the balances of the accounts for the Community Superpeers).

How will we know which accounts are on RightMesh? The RightMesh Superpeers will have an event fire when peers join the network for initial transfer. Depending on if the device joining the network is a Superpeer or a remote device, different initial balances are sent.

![How the faucet fits into the RightMesh network topology with Community Superpeers, RightMesh Superpeers and local meshes. When new devices join, appropriate funds for operation during the competition are automatically provided.](/assets/img/faucet2.jpeg)

The RightMesh Superpeers will also be running threads which check to see if the balance is below a threshold (of ETH or RMESH). If the balance is too low, the RightMesh Superpeer will send testnet ETH and testnet RMESH to the required device. The main attack that could occur here is that an end user could continually send its funds somewhere else — just to cause the RightMesh Superpeers to run out of testnet funds. The RightMesh Superpeers will keep track of when the last payout happened and if it has been too soon, this device will be blacklisted. The user of this device will have to contact us to continue to participate in the competition.

The final post in this series will break down into greater detail how a potential payment channel balancing algorithm may work in the context of this automated faucet.

**Tasks:**
* Accumulate sufficient Superpeer funds by mining on the testnet (or working with testnet operators to obtain enough testnet ETH for the competition period).
Implement balance transfer for Superpeers and Remote Peers (this should not be done in the open source part of the Superpeer — but in an added library like the visualisation code).
* Implement Superpeer thread which checks for low balances and transfers to-up
* Implement blacklisting checks for devices transferring their ETH away. (persist this into a database so if Superpeers go offline the blacklist isn’t lost. Have all RightMesh Superpeers use the same database. Perhaps have the database on an additional set of servers all RightMesh Superpeers can access).
* Test with a Single Superpeer and ensure that the auto transfers work
* Test with multiple RightMesh Superpeers
* Test with Community Superpeers
* Update the visualisation Superpeer so that it can show token balance and ETH balance of all devices on RightMesh network along with corresponding payment channel balances.

This post originally appeared on Medium: [Multiple Superpeers Post 7/8: Automatic Faucet](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-6-automatic-faucet-a3ab3a8a3d73)
