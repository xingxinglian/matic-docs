---
id: approve
title: approve
keywords:
  - 'plasma client, erc721, approve, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`approve` 메소드는 루트 토큰에 필요한 수량을 승인하는 데 사용할 수 있습니다.

```
const erc721RootToken = plasmaClient.erc721(<root token address>,true);

const approveResult = await erc721RootToken.approve(<token id>);

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
