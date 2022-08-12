---
id: approve-all
title: approveAll
keywords:
  - 'pos client, erc721, approveAll, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`approveAll` 메소드는 모든 토큰을 승인하는 데 사용할 수 있습니다.

```
const erc721RootToken = posClient.erc721(<root token address>, true);

const approveResult = await erc721RootToken.approveAll();

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
