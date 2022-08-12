---
id: approve-max
title: approveMax
keywords:
  - 'plasma client, erc20, approveMax, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# approveMax

`approveMax` 메소드는 루트 토큰의 최대 수량을 승인하는 데 사용할 수 있습니다.

```
const erc20RootToken = plasmaClient.erc20(<root token address>, true);

const approveResult = await erc20Token.approveMax();

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
