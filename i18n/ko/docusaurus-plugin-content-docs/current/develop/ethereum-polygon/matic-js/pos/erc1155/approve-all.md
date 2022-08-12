---
id: approve-all
title: approveAll
keywords:
  - 'pos client, erc1155, approve, polygon, sdk'
description: 'erc1155 토큰 승인'
---

# approveAll

`approveAll` 메소드는 모든 토큰을 승인하는 데 사용할 수 있습니다.

```
const erc1155RootToken = posClient.erc1155(<root token address>,true);

const approveResult = await erc1155RootToken.approveAll();

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
