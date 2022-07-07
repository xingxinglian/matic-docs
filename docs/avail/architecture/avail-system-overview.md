---
id: avail-system-overview
title: System Overview
sidebar_label: System Overview
description: Learn about the architecture of the Avail chain.
keywords:
  - docs
  - polygon
  - avail
  - data
  - availability
  - architecture
image: https://matic.network/banners/matic-network-16x9.png
slug: avail-system-overview
---

## Modularity

Currently, monolithic blockchain architectures like that of Ethereum 
cannot efficiently handle execution, settlement, and data availability. 

Using modularity to scale blockchains is what the rollup-centric or 
application-specific chain model attempt to do. Sometimes, this works 
best when the settlement and data availability layer are on the same 
layer, which is the approach Ethereum rollups take.

However, a granular design is designing different layers to be lightweight 
protocols, like microservices. Then, the overall network becomes a collection 
of loosely-coupled lightweight protocols. An example is a data availability 
layer that only specializes in data availability. Polygon Avail is a Substrate-based 
application-specific chain for DA (data availability). 

:::note Substrate runtime

Modifications have been made to the actual Substrate runtime as opposed to 
the standard plug-and-play approach Substrate's FRAME offers. Please see the 
[Avail architecture guide] for more details.

:::

Avail provides a high guarantee of data availability to any light client. 
Avail makes it possible to prove that block data is available without
downloading the whole block by leveraging Kate polynomial commitments,
erasure coding, and other technologies to allow light clients (which
download only the _headers_ of the chain) to efficiently and randomly
sample small amounts of the block data to verify its full
availability. However, there are fundamentally different primitives than 
standard fraud-proof-based DA systems, which are explained [here].

### Enabling the next set of solutions

There are, of course, trade-offs that are made with different modularity 
approaches, but the overall goal is to maintain high security while being able 
to scale. Avail will take rollups to the next level as chains can allocate 
their data availability component to Avail. Avail also provides an alternative 
way to bootstrap [any] standalone chain, as chains can offload their data 
availability. Transaction costs are also reduced; the primary cost 
of committing a transaction through a rollup is the data availability cost for 
putting transaction data directly onto a layer one. Avail also makes sovereign 
rollups a possibility. Sovereign rollups on Avail allow for seamless upgrades, 
as updates can be pushed to application-specific nodes to upgrade the chain and, 
in turn, upgrade to new settlement logic. Whereas in a traditional environment, 
direct updates would need to be made to contracts, or the network requires a fork.

:::info Avail does not have an execution environment

Avail does not run smart contracts, but it makes it possible
for other chains to make their transaction data available through Avail.
These chains may implement their execution environments of any kind,
EVM, Wasm, or anything else.

:::

:::info Avail doesn't care what the data is

Avail guarantees that block data is available but does not care about
what that data is. The data can be transactions but can take
on other forms too. Data availability on Avail is available 
for a window of time that it is required. For instance, beyond needing
data or reconstruction, security is not compromised.

Storage systems, on the other hand, are designed to store data for long
periods, and include incentivization mechanisms to encourage users
to store data.

:::

## Validation

### Peer validation

Typically, Ethereum-like ecosystems contain three types of peers:

* **Validator nodes:**
  A validator collects transactions from the mempool, executes them, and generates a 
  candidate block that is appended to the network. The block contains a small block 
  header with the digest and metadata of the transactions in the block.
* **Full nodes:**
  The candidate block propogates to full nodes across the network for verification. 
  The nodes will re-execute the transactions contained in the candidate block. 
* **Light clients**
  Light clients only fetches the block header to use for verification, and will fetch 
  transaction details from neighboring full nodes as needed.

While a secure approach, Avail addresses the limitations of this architecture to create 
robustness and increased guarantees. Light clients can be tricked into accepting blocks 
whose underlying data is not available. A block producer can include a malicious transaction 
in a block and not reveal its entire content to the network. As mentioned in the Avail
documentation, this is commonly known as the data availability problem.

Avail's network peers include:

* **Validator nodes:**
  Protocol incentivized full nodes that participate in the consensus.

* **Avail (DA) full nodes:**
   Nodes that download and make available all block data for all applications
   using Avail.

* **Avail (DA) light clients:**
   Clients that only download block headers, which randomly sample small parts of 
   the block to verify availability. They expose a local API to interact with the 
   Avail network.

:::info The goal of Avail is to not be reliant on full nodes to keep data available

  The aim is to give the same DA guarantees to a light client as a full node. 
  Users are encouraged to use Avail light clients. However, users are able to run Avail 
  full nodes and it is well supported.

:::

:::caution The local API is a WIP and is not yet stable
:::

This allows applications that wish to use Avail to embed the DA light
client. They can then build:

* **App full nodes:**
  - Embed an Avail (DA) light client
  - Download all data for a specific appID
  - Implement an execution environment to run transactions
  - Maintain application state

* **App light clients:**
  - Embed an Avail (DA) light client
  - Implement end user facing functionality

The Avail ecosystem will also feature bridges to enable specific
use-cases. One such bridge being designed at this time is an
_attestation bridge_ that will post attestations of data available on
Avail to Ethereum, thus allowing validiums to be built.

## State verification

### Block verification -> DA verification

Instead of Avail validators verifying the application state, they concentrate 
on ensuring the availability of posted transaction data and provides transaction 
ordering. A block is considered valid only if the data behind that block is available.

Requiring data to be available prevents block producers from releasing block headers
without releasing the data behind them, as this prevents clients from reading the 
transactions necessary to compute the state of their applications. Avail uses data 
availability verification to address this through DA checks which utilize erasure 
codes; these checks are heavily used in data redundancy design.

DA checks require each light client to sample a very small number of random chunks 
from each block in the chain. A set of light clients can collectively sample the 
entire blockchain in this manner. Consequently, the more non-consensus nodes there are 
the greater the block size (and throughput) can securely exist. Meaning, non-consensus 
nodes can contribute to the throughput and security of the network.

### Transaction settlement

Avail will use a settlement layer built with Polygon Edge. The settlement layer 
provides an EVM-compatible blockchain for rollups to store their data and perform 
dispute resolution. The settlement layer utilizes Polygon Avail for its own DA. 
When rollups are using a settlement layer, they also inherit all the 
DA properties of Avail.

Avail offers data hosting and ordering. The execution layer will likely come from 
multiple off-chain scaling solutions or legacy execution layers. The settlement
layer takes on the verification and dispute resolution component.

## Resources

- [Introduction to Avail by Polygon blog post](https://medium.com/the-polygon-blog/introducing-avail-by-polygon-a-robust-general-purpose-scalable-data-availability-layer-98bc9814c048).
- [Polygon Talks: Polygon Avail]
