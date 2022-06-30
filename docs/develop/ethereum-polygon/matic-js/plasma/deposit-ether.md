---
id: deposit-ether
title: deposit
keywords: 
- 'plasma client, depositEther, polygon, sdk'
description: 'Deposit required amount of **ether** from ethereum to polygon.'
---

`depositEther` method can be used to deposit required amount of **ether** from ethereum to polygon.

```
const result = await plasmaClient.depositEther(100);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
