---
id: withdraw-exit-many
title: withdrawExitMany
keywords:
  - 'pos client, erc1155, withdrawExitMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawExitMany` 메소드는 `withdrawStartMany` 메소드의 txHash를 사용하여 인출 프로세스를 종료하는 데 사용할 수 있습니다.

```
const erc1155RootToken = posClient.erc1155(<root token address>, true);

const result = await erc1155RootToken.withdrawExitMany(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
