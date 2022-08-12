---
id: withdraw-exit-many
title: withdrawExitMany
keywords:
  - 'plasma client, erc721, withdrawExitMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawExitMany` 메소드는 모든 토큰을 승인하는 데 사용할 수 있습니다.

```
const erc721RootToken = plasmaClient.erc721(<root token address>, true);

const approveResult = await erc721RootToken.withdrawExitMany(<burn tx hash>);

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
