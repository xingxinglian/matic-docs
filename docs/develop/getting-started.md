---
id: getting-started
title: Developing on Polygon PoS
sidebar_label: Quick Start
description: Build your next blockchain app on Polygon.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

:::caution Updating the Develop Docs

The docs are being updated, enhanced, and improved. They are subject to change.
Please feel free to raise an issue or pull request if you have any queries or suggestions.

:::

Welcome to the most exciting and innovative platform to build your blockchain application on **Polygon Blockchain**. Blockchain technology is poised to revolutionise the way the digital world handles data and does business. Be a part of this revolution and get a head start with decentralised application development on Polygon.

This page will act as your guide into the **Polygon Ecosystem**. You'll find links to some useful resources and websites to get you up and started with building on Polygon in particular and Blockchain in general. Feel free to reach out to us on [Telegram](https://t.me/polygonofficial) or [Discord](https://discord.com/invite/tCmCbEff66).

## **Developer Quick Start**

If you're an Ethereum Developer, you're already a Polygon developer.
All the tools you're familiar with are supported on Polygon out of the box: Truffle, Remix, Web3js.
Switch over to Polygon's RPC and get started!

Polygon's Test Network which is called **Mumbai** connects with **Ethereum's Goërli Testnet.**
All the network related details can be found in [network docs](/docs/develop/network-details/network).

- Set up [Metamask Wallet](/docs/develop/metamask/overview) or [Arkane Wallet](/docs/develop/wallets/arkane/intro)
- Deploy your Contracts on Polygon
    - [Using Alchemy](/docs/develop/alchemy)
    - [Using Chainstack](/docs/develop/chainstack)
    - [Using QuickNode](/docs/develop/quicknode)
    - [Using Remix](/docs/develop/remix)
    - [Using Truffle](/docs/develop/truffle)
    - [Using Hardhat](/docs/develop/hardhat)
    - [Using Replit](/docs/develop/replit)
- Connecting to Polygon [with RPC](https://docs.polygon.technology/docs/develop/metamask/config-polygon-on-metamask) by adding Polygon on Metamask or directly via [Arkane](/docs/develop/wallets/arkane/network).

:::note
You can use the same RPC with web3.js.
:::

```jsx
// Javascript
const Web3 = require('Web3')

// Sign up for a free dedicated RPC URL at https://rpc.maticvigil.com/ or other hosted node providers.
const web3 = new Web3('https://rpc-mumbai.matic.today')

// Sign up for a free dedicated RPC URL at https://www.alchemy.com or other hosted node providers.
const web3 = new Web3('https://polygon-mainnet.g.alchemy.com/v2/<your-api-key>')

// web3 object is now connected with Polygon's node
```

## Current Sections

- If you need to know how to set up a node, its technical requirements and network endpoints head to [Nodes](network-details/technical-requirements).
- For key management for dapps, integration for dapps with wallets and how to use Polygon Web Wallet, go for [Wallets](wallets/getting-started).
- For instructions on how to install, set up and implement Matic.js library, head to [Matic.js](ethereum-polygon/matic-js/get-started).
- You can find information about tokens and NFTs at [Assets](network-details/gas-token).
- Developing or deploying smart contracts? Start [here](alchemy).
- The [transactions](eip1559-transactions/how-to-send-eip1559-transactions) category is useful for finding information about how to send EIP1559 transactions, as well as learning about meta transactions and how to integrate them with your dApp.
- Know more about the cross-layear communication between Polygon and Ethereum on the [Bridges](ethereum-polygon/getting-started) page.
- Read about how to use [Oracles](oracles/getting-started) to query and fetch data when developing a dApp on Polygon.  
- This documentation also provides information for [data storage](ipfs) using IFPS and Filecoin for instance.
- To handle data indexing and querying, go to [Data](graph).
- You can also learn about decentralized identifiers (DID) and how to implement them on the page [Identity](did-implementation/getting-started) page.
- We also provided some [extra resources](tools/polygon-faucet) for developers, namely tools and relevant links.  

### **Already have a dApp?**

- **Migrating from Ethereum chain (or any EVM based chain for that matter)**

    Deploy all your smart contracts directly on Polygon chain. You don't have to worry about the underlying architecture, as long as it is EVM compatible.

    [Deploying your dApp on Polygon](https://docs.polygon.technology/docs/integrate/quickstart)

- **Using Polygon as a faster transactions layer**

    Using Polygon as a transactions layer in your dApp deployed on Mainnet, you can get started with getting your tokens mapped by us.

    Getting your tokens mapped on Polygon: 👋🏼 Ping us on [http://bit.ly/matic-technical-group](http://bit.ly/matic-technical-group)

### **Building a new dApp on Polygon?**

**Start building!**

- [Full Stack dApp: Tutorial Series](https://kauri.io/full-stack-dapp-tutorial-series/5b8e401ee727370001c942e3/c)
- Getting to know your tools:

    - [Web3js](https://www.dappuniversity.com/articles/web3-js-intro), [Ethers.js](https://docs.ethers.io/v5/),[Remix](https://docs.polygon.technology/docs/develop/remix/), [Truffle](https://docs.polygon.technology/docs/develop/truffle), [Metamask](/docs/develop/metamask/overview), [Arkane](/docs/develop/wallets/arkane/intro)
- [Develop a dApp using Fauna, Polygon and React](/docs/develop/dapp-fauna-polygon-react)

### **Get Involved with Hackathons**

Hackathons take place at blockchain conferences and meetups all over the world. Chances are you can’t hop on a plane on a moment’s notice, but some conferences or projects host virtual hackathons open to anyone with an internet connection. Check [Polygon's Hackathons Discord Channel](https://discord.gg/polygon)

## Pointers

If this is overwhelming, that’s okay! You can jump right into the fire and start hacking. Here are a few pointers before you start diving into resources, repositories, and documentation.

1. **Beware the cost of being on the bleeding edge.** More so than typical niche programming, dapp and blockchain development moves very quickly. Deep into learning, you may find complex code repositories, 404s on a documentation site, or, perhaps, no documentation at all. Rather than seeing this as a deterrent, see it as an invitation to an **Opportunity**. Ping on our developer channel, find the Discord / Gitter / Telegram channel, post on Stack Overflow or Reddit — you may be surprised at the rate of response and openness of the community.
2. **The learning curve may be daunting, but the barrier to entry is low**. All communities have their grumps, of course, but if you do the work, put in the effort, it will be noticed. Projects welcome pull requests from outsiders and support will be there if you’ve exhausted every other resource. We’re working on creating a better world and can use all the help we can get. We’re just glad you’re here.

## Keep up with up with Development
Dapp development encourages network decentralization, and it also embodies it. As developers are located all over the world, the best way to stay connected with Polygon's development is by following our Social Media. All links to [Polygon community can be found here](https://polygon.technology/community/).
