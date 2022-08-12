---
id: deposit-ether
title: deposit ether
keywords:
  - 'pos client, depositEther, polygon, sdk'
description: 'depositEther 메소드는 이더리움에서 폴리곤에 필요한 수량의 이더를 입금하는 데 사용할 수 있습니다.'
---

`depositEther` 메소드는 이더리움에서 폴리곤에 필요한 수량의 **이더**를 입금하는 데 사용할 수 있습니다.

```
const result = await posClient.depositEther(<amount>, <userAddress>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
