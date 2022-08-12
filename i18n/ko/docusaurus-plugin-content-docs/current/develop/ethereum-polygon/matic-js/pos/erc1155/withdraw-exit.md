---
id: withdraw-exit
title: withdrawExit
keywords:
  - 'pos client, erc1155, withdrawExit, polygon, sdk'
description: 'withdrawExit 메소드는 withdrawStart 메소드에서 txHash를 사용하여 인출 프로세스를 종료하는데 사용할 수 있습니다.'
---

`withdrawExit` 메소드는 `withdrawStart` 메소드에서 txHash를 사용하여 인출 프로세스를 종료하는데 사용할 수 있습니다.

```
const erc1155RootToken = posClient.erc1155(<root token address>, true);

const result = await erc1155RootToken.withdrawExit(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
