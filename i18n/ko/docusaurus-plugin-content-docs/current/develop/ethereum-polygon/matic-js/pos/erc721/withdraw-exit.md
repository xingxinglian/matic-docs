---
id: withdraw-exit
title: withdrawExit
keywords:
  - 'pos client, erc721, withdrawExit, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawExit` 메소드는 `withdrawStart` 메소드에서 txHash를 사용하여 인출 프로세스를 종료하는 데 사용할 수 있습니다.

```
const erc721RootToken = posClient.erc721(<root token address>, true);

const result = await erc721RootToken.withdrawExit(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```


이 메소드는 여러 RPC 호출을 수행하여 증명 및 프로세스 종료를 생성합니다. 따라서 withdrawExitFaster 메소드를 사용하는 것이 좋습니다. >