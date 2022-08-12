---
id: deposit
title: deposit
keywords:
  - 'pos client, erc20, approveMax, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# deposit

`deposit` 메소드는 루트 토큰에서 하위토큰으로 필요한 수량을 입금하는 데 사용할 수 있습니다.

```
const erc20RootToken = plasmaClient.erc20(<root token address>,true);

//deposit 100 to user address
const result = await erc20Token.deposit(100, <user address>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
