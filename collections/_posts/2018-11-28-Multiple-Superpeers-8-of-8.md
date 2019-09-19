---
layout: default
title: "Multiple Superpeers 8/8: Payment Channel Balancing Algorithm"
published: true
featured-image: "roadmap8.png"
featured-alt: ""
categories: [Technical Roadmap, Wireless Networks]
tags: [RightMesh, Payment Channels, Blockchain, Ethereum, Mesh Networks]
---

This is the final post in the RightMesh Roadmap: Multiple Superpeers series. Links to the previous posts can be found at the bottom of the article. While most posts focused around the protocol aspects of making Superpeers work, this post and the previous one ([Automatic Faucet](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-6-automatic-faucet-a3ab3a8a3d73)) focus on tokens and Ethereum — specifically how they are provided to end-users, how they are managed by Superpeers, and how economics fits in with the costs of establishing, funding, re-balancing, and withdrawing from in relation to data moving through the RightMesh network.

The payment channel balancing algorithm is a critical component of Superpeers’ revenue-generating potential in the RightMesh network. Given a fixed number of tokens, Superpeers are only able to distribute the funds in a fixed number of channels and still make profit. There are costs required to open channels, fund channels, top-up channels and withdraw funds. This post outlines some of the issues we are considering for our reference implementation of the default “payment channel balancing” algorithm which will be provided to Community Superpeers for the competition. The algorithm will also be provided as open source so that developers in the competition may further build on it to create a more efficient, and potentially more profitable, approach.

There is a yet-to-be-defined minimum amount of funds that must move across a channel before the channel can become profitable. This is based on the ETH price, the RMESH token price, and the fiat currency that people are purchasing the data sold into the network in (as well as to a lesser extent the fiat currency used by the Superpeers to pay for running the server — electricity, bandwidth costs, cpu costs, depending on if the server is run locally or via a service like AWS, Google or Azure).

Given that there is a minimum amount of funds which must move across a channel to become profitable, and a fixed number of tokens available, the Superpeer must become very good at selecting which Sellers to make payment channels with and which Buyers to cash payment channels out from.

The goal of this algorithm is to determine:

1. How do we support the maximum number of concurrently open payment channels with the highest profit?
2. How can we “re-balance” with low delay given a fixed number of tokens without losing money due to the cost of establishing, funding, and withdrawing from payment channels?
3. How do we initially assign tokens; when do we close a channel?
4. Do we close channels on the Remote Peer -> Superpeer side, the Superpeer-> Seller Side, or both?

We are assuming that the Buyer must pay for deploying the payment channel from the Buyer to the Superpeer in ETH and must also fund the channel in tokens. The Buyer is running an algorithm which minimises their cost, and we desire there to be little friction for the Buyer; for example, we only want them to have to have RMESH tokens, and very little ETH if any — they certainly shouldn’t have to keep accumulating it.

We are assuming the Superpeer must pay for deploying the channel between the Superpeer and the Seller in ETH and must also fund the channel in tokens. This means if Sellers close the channels too often to recover their funds, the Superpeer will also lose money. For the Mainnet Soft Launch we will assume that the Sellers never need to do this. In practicality, for the real functioning system, we will need to keep this in mind and either

a) prevent Sellers from closing down before the Superpeer is profitable, or
b) blacklist on the Superpeer side Sellers who close too early.

To illustrate the problems with some examples, let’s assume the following case:
We have a Superpeer with a limited supply of RMESH, and some sellers. The Superpeer decides to create payment channels to each Seller with the same number of tokens for each. At some point, if enough Sellers join, the Superpeer will clearly run out of funds.

![An example of an initial distribution of funds into payment channels from Superpeer to Data Sellers. No Buyers have yet joined the network.](/assets/img/channel-balancing.jpeg)

However, as in the example above, none of the Sellers have any Buyers. Let’s assume that the Sellers will eventually get some Buyers. As in the following Figure, the Buyers establish payment channels to the Superpeer which is acting as a payment hub. As the Buyers use data, the payments move from the Buyer to the Superpeer, and additionally, some of the funds will move from the Superpeer to the Seller. Eventually, all of the funds will accumulate on one-side of a Buyer-Superpeer channel or a Superpeer-Seller channel. This is called a skewed channel.

![Another example of an initial distribution of payment channel funds with Buyers and Sellers involved with a Superpeer acting as a payment hub.](/assets/img/channel-balancing1.jpeg)

The next figure shows all the channels being skewed, and what’s worse, the Superpeer has no reserve funds left to fix the problem. The only solution is to withdraw from the Buyer channels and use the funds to continue to fund the Seller channels. This also requires the Buyers to top-up their payment channels. In the Soft Mainnet Launch, this top-up will happen automatically. In the Hard Mainnet Launch (to follow the Soft Mainnet Launch), we will make the assumption that there will be many more possible Buyers, and they will find enough value in using RightMesh to continue purchasing RMESH to use the network.

![One example of a balancing problem. The Superpeer is out of funds and needs to top-up the payment channels towards the Data Sellers. Which Buyer channel should be closed?](/assets/img/channel-balancing2.jpeg)

Another challenging case is the following:

Suppose the Seller channels need to be topped up but there is not a clear Buyer channel to recoup the funds from. Is it better to close the Buyer channel with 3 or the Buyer channel with 5? Also, suppose there is a new Seller who wishes to join the network. Should the reclaimed funds be used to fund continued use of the established channels or the new one?

![A slightly different example of the problem — there are differing amounts remaining in the Buyer Channels, and differing amounts yet to paid toward the Superpeer. Which should be selected? Should the new channel be created?](/assets/img/channel-balancing3.jpeg)

To give one possible solution to the payment channel re-balancing problem lets consider the following:

**Initial Assignment**
This approach is very simple. The Superpeer makes outgoing payment channels only between itself and the Remote Peers which are directly connected to the Internet.

All Remote Peers make payment channels to the Superpeer. For the sake of this discussion, let’s assume the Superpeer pays for the channels the first time they are created, but if the Remote Peer wants to close to take its balance back, it must pay for any subsequent re-opens. Similarly, the Superpeer will pay for the initial open of a channel between the Superpeer and the Seller. All smart contract operations cost ETH, but the payment channels themselves have tokens deposited in them.

If a Remote Peer does not have a direct connection to the Internet, a payment channel will not be made between the Superpeer and the Remote Peer (but reverse is true). This will save some tokens, allowing it to scale in terms of “number of meshes” it supports, rather than in terms of number of nodes connected either directly or through a Superpeer. In other words, the Superpeer is not making both Buyer and Seller payment channels to all nodes as it currently works in our implementation. It will only make a Seller payment channel for nodes it receives an INFO packet from directly (non-relayed).

As a starting point, let’s assume that each channel (Buyer and Seller) will have 1 RMESH assigned into it from the Superpeer side. This means at most, it can support 1000 payment channels (given our assumption of a fixed amount of 1000 tokens available per Superpeer) before it starts rejecting Remote Peers. In effect it also means it can support 1000 “meshes” or devices directly connected to the Superpeer. A downside is that when a device connects once and obtains a payment channel, it permanently keeps those resources. A simple modification to prevent this may be a timeout mechanism or closing the channel when the Remote Peer disconnects from the Superpeer (although one would need to make sure the Remote Peer is not experiencing intermittent connectivity which could cause the Superpeer to spend lots of ETH re-establishing channels). Perhaps a further revision could be to re-open a channel with a particular Remote Peer with less and less likelihood the more often this needs to be done in a short period of time (ie: prioritize Remote Peers with more stable Internet connections).

Let’s assume the Remote Peer can add to its balance much more cheaply than it costs to fully re-open a channel. Similarly, the Superpeer can reload the balance between itself and a Seller cheaply.

Let’s also assume the Superpeer can set a “fee” that covers some of the re-balancing cost. For now, let’s assume the fee is fixed (in future it can be tied to things like exchange rates, actual data costs of running the Superpeer, electricity costs, etc.)

**Ongoing Re-balancing**
Channels would be re-balanced (expensive) under the following circumstances:

1. Remote Peer leaves, there is some balance on the Superpeer side of the payment channel and a) the Remote Peer has been gone for a long enough time or b) there is a sufficient balance to warrant profit by cashing out.

2. Seller leaves, there is some balance on the Superpeer side, and the Seller has been gone for long enough to warrant cash out with profit.

Channels would (cheaply) be reloaded without closing under the following circumstances:

1. Remote Peer -> Superpeer channel is running low and the Remote Peer is still active.

2. Superpeer -> Seller is running low and there are still active Remote Peers.

## Evaluation Criteria:

**1. Complexity**: Fairly simple initial distribution, more complex re-balancing.
**2. Cost over time**: Initial setup = 1000 * gas for contracts.
**3. Ongoing cost**:

For the remaining calculations, let’s assume the Superpeer->Seller channel is skewed entirely towards the Seller. (Note that this section was written in August — so the calculations need to be redone with current ETH and RMESH prices. In practice, any implementation would continually look at current prices in order to inform the algorithms.)

If we assume each Data Seller has only a single Buyer and we don’t want to close any of the non-operational channels, it will cost the gas to close the payment channel between the Buyer and the Superpeer, the gas to re-open the channel and the gas to reload the channel between the Superpeer and the Seller. The revenue for this is at most 1 token + whatever the fee is.

C = close + reopen + reload + 1 token
R = 1 token + fee

Doing this operation only makes sense if the revenue is high enough from the fee.
Unfortunately, this is the best-case scenario. If we allow that there may be many Buyers, the Superpeer-> Buyer channel will run out of money far before any of the Buyers will actually send a full token — on average the amount each will have in the Superpeer side of the channel at the time re-balance is needed is: ( 1 / number of Buyers ).

So, now there are two options:

1. Pick the Buyer with the highest amount in his/her payment channel on the Superpeer side, or
2. Close all the payment channels headed towards the particular Seller.

Probably the second option is not good because the closing and re-opening cost is too high.
If we pick closing a single one:
C = close + reopen + reload + [(1 / number of Buyers) * (tokens per channel)]
R = [(1 / number of Buyers) * (tokens per channel)] + fee

*(note, formula has been adjusted to be more general to any number of tokens per payment channel)*

It doesn’t really matter how many Buyers we have in particular, because if we assume that we are only cashing out one payment channel at once, we have the same number of tokens in revenue as in cost going out to the Seller channel.

We may find that the fee must be much larger than the number of tokens that will ever be in the payment channel when we plug in the real ETH amounts for the closing, reopening and reloading values; however, there are a few things we can do to address this:

1. Increase the number of tokens per payment channel without increasing the overall number of tokens per Superpeer (ie: support fewer simultaneous channels but have more in each)
2. Keep the number of channels we support constant but increase the overall number of tokens we make available to each Superpeer.

To be able to calculate ongoing costs for the Superpeer, we also need to know how often the payment channels need to be re-balanced and reloaded. This is quite complex; however, we can make assumptions to make it easier.

**Ongoing cost using India as a potential market:**
Currently, the price of RMESH is around $0.10. Two GB one-time use on the most affordable plan with [Jio in India is worth 98 rupees](https://www.jio.com/en-in/4g-plans): — which works out to $1.40USD ~ 14 RMESH or $0.70USD per GB. (For this exercise, we’ll assume people are willing to sell at the same price they bought just to unload it although — it may be more or less, in reality).

So, a 1 RMESH payment channel would get 0.1429 GB of data or 143 MB of data (minus the fees). Let’s assume Data Seller provides this much data to the RightMesh network every day.

According to our calculations during faucet planning, 0.001 ETH may be required to open a payment channel, or about $0.28USD at the August 2018 exchange rate. Let’s assume that reloading the channel costs 10x less than that, so 0.0001 ETH or $0.028USD. Let’s also assume that closing is half the cost of the opening cost, so 0.14USD.

So fees = 0.14 + 0.28 + 0.028 = 0.448 USD

Let’s assume we want the fees to be less than 10% of the total amount of the data cost moving through. That means we need to move $4.48 USD of data. Further, we want the Superpeer to be able to set their fee to be 10% of the total they just facilitated, so this number should rise to $4.93USD. This means for every channel where the full balance has been paid out to the Superpeer, $0.45USD is made by the Superpeers (or about 4.5RMESH). They will have made this by staking ~ 45 RMESH in the payment channel.

To use that much data; however, people need to move to a more expensive plan, as the minimum plans won’t suffice. If we go to the 2GB per day plans (with a total of 56GB over 28 days), the cost is 198R ~ $2.83USD. If you cost this over the whole 28 days, it works out to only $0.05 per GB. If we assume the Data Sellers are selling a break-even option, it is impossible with this particular data plan to restrict to only 10% of the total value of the data. However, let’s look at some of the smaller plans from Jio to get an idea of what people will pay for smaller bundles.
If we look at the sachet prices with Jio, there is one plan for 1.05GB for 52R ~ 0.74USD per GB (10 times more) or 19R for 0.15GB ~ $1.8USD per GB (27 times more).

So, let’s assume Data Sellers want to give a deal that is cheaper than Jio’s smaller bundles, but higher than what it is costing them. Let’s assume they want to sell at a rate of $0.40USD per GB. Let’s also assume they sell their entire 2GB for the day at full 4G speed, and that they continue to sell another 250MB at the reduced throughput of 64kbps (taking 9 hours while they sleep). They would have made $0.9USD of revenue. If we want to reach the $4.93USD for the 10% fee ratio to take effect, a Data Seller needs to sell **5.47 days worth of data**, or 12GB of data.

Keep in mind, that the Data Seller is making quite a bit of money here — once they reach the $2.83 USD, everything else is profit, and they can sell up to 56GB. With the 10% of the 0.40 going to the Superpeers, they are earning $0.36USD per GB. They reach break even if they sell 7.86GB of their 56GB. At this point, they essentially can use their data plan for their own use for the rest of the month, or they can continue selling at the same rate for a profit of **$17USD per month**.

How much will the Superpeer make in this scheme? Let’s suppose for now that the Superpeer has zero bandwidth cost and unlimited throughput. The Superpeer is essentially making $0.04 per GB; however, in theory, it can support 1000 payment channels at once (assuming we extend the number of tokens a Superpeer can use for the pool). With all of the possible Data Sellers selling 56GB into the network each month, that works out to 56TB of data per month. This works out to $2240USD or 22400 RMESH earned off 450000 staked RMESH. 56TB per month ~ 170Mbps. Of course, this number can be increased by raising the fee, or by becoming more efficient at balancing payment channels, or eventually with third party services built on-top of Superpeers such as content or ad distribution.

**4. Maximum Payment Channels Per Superpeer**
One thousand, or a lower number if we want to keep the number of tokens constant, but need to adjust the number of tokens per channel to make a profit.

**5. Average Funds Per Payment Channel**
The Superpeer->Seller channels will always have a balance of at least 1 RMESH (or at least the tokens per channel value). If the Sellers don’t cash out, this value will steadily rise as the Superpeers push more and more tokens into them as the funds move to the Seller side.

On the Buyer -> Superpeer side, this can essentially be unlimited; however, the bottleneck is the Superpeer->Seller side, because it makes no sense to continue to send data towards the Superpeer if the Superpeer can’t pay the Seller. The Superpeer can’t make advances to the Seller because the funds need to be in the payment channel in the first place before a signed transaction will mean anything (unless we want to just have Sellers “trust” that the Superpeer will follow up with unpaid balances but we do not believe this is a viable idea).

According to above analysis, **45** is the number of tokens that should be in the payment channel. Alternatively, we propose we could make many channels with the smallest amount possible, and if they are re-loadable, just reload them when they are used so less is tied up.

**6. Average Revenue Per Time Period**
This is driven solely by the fees, and how quickly data is consumed. According to assumptions in #2 above, this would be $2240 USD (or $4480 if I can find the mistake), based off 56TB of bandwidth per month. This is assuming a 10% fee for the Superpeer.

**7. Break even time:** 5.47 days

**8. Total Staked Tokens per Superpeer:** 450,000

**9. Total Staked for 1000 Superpeers:** 450,000,000

One of the weaknesses of this approach is that it takes 450,000 RMESH per Superpeer to support 1000 meshes, each with at most 30 Buyers. If we want to support 1000 Superpeers, it would require a supply of 450,000,000 RMESH. That would give us 30,000,000 nodes, but unfortunately, we only have 129,000,000 RMESH. This demonstrates how the price of RMESH ties into these models. If the price falls too low, there is not enough supply for the Superpeers, which should drive the price up. For instance, if the price goes from 0.10USD RMESH to 1.10 USD RMESH the supply required is 45,000,000, if the prices goes to 10.10 RMESH the required supply should be 4,500,000.

**Strengths**
Simple implementation. Reduced payment channels compared to establishing Buyer and Seller channel for all nodes. Can be incrementally improved to address shortcomings.

**Weaknesses**
Fixed scalability. Some delay between when a device switches from Buyer to Seller and payment channel being established (compared with both payment channels being established at same time). Delay to perform closing, reloading, etc

**Tasks:**
* Update smart contract to allow for topping up, withdrawing and funding channels.
Implement a slider in UI for Seller which, instead of price, slides between “not willing” and “willing” to share data. Have this translate into a price that goes very high and a price that goes very low.
* Alternatively, implement an input item which allows a Seller to answer a question of how much their data costs per GB. Allow the slider to control the markup they wish to make on-top of this, make sure they understand they will not actually be earning this (since they will just be earning testnet tokens in the competition, with the chance of earning real RMESH).
* Implement the ability for the Superpeers to set their fees in terms of a percentage. Not sure whether this should be based on the raw data price, or the data price + the Seller markup.
* Implement strategy for distributing the initial tokens to Sellers (until the tokens run out and balancing is required). To do this, the Superpeer will need to know in ETH how much it expects to earn on the percentage. It will also need to estimate based on current gas price how much ETH will be required to run the close, reopen, reload — so functions to do these will need to be implemented.
* Implement response for Buyer request to open a payment channel with <x> funds. The response should be based on whether enough funds have been requested, which should be worked out ahead of time, based on the Seller price of data, the price of ETH and the price of RMESH.
* Implement the balancing algorithm for when tokens run out. First focus on supporting the same 1000 Sellers by only making sure tokens continue to flow (closing Buyer side channels to recoup the money and pass it into Seller side). Second focus on how to support potential new Sellers by closing down channels with existing Sellers who aren’t producing.
* Implement the request to notify Sellers that the Superpeer intends to continue to fund the channel and they should keep it open.

This post originally appeared on Medium: [Multiple Superpeers 8/8: Payment Channel Balancing Algorithm](https://medium.com/rightmesh/rightmesh-roadmap-multiple-superpeers-part-7-payment-channel-balancing-algorithm-35e7f5811fe2)
