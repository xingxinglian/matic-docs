---
id: withdraw-exit-many
title: withdrawExitMany
keywords:
  - 'pos client, erc721, withdrawExitMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawExitMany` 메소드는 `withdrawStartMany` 메소드의 txHash를 이용하여 인출 프로세스를 종료하는 데 사용할 수 있습니다.

```
const erc721RootToken = posClient.erc721(<root token address>, true);

const result = await erc721RootToken.withdrawExitMany(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
