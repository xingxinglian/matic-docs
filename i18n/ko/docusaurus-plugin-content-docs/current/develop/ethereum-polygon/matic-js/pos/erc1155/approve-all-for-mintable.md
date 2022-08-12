---
id: approve-all-for-mintable
title: approveAllForMintable
keywords:
  - 'pos client, erc1155, approve, polygon, sdk'
description: 'erc1155 토큰 승인'
---

# approveAllForMintable

`approveAllForMintable` 메소드는 루트 토큰의 모든 발행 가능 토큰을 승인하는 데 사용할 수 있습니다.

```
const erc1155RootToken = posClient.erc1155(<root token address>,true);

const approveResult = await erc1155RootToken.approveAllForMintable();

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
