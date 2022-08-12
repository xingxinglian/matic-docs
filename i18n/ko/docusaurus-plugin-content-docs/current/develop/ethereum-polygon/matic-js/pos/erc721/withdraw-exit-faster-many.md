---
id: withdraw-exit-faster-many
title: withdrawExitFasterMany
keywords:
  - 'pos client, erc721, withdrawExitFasterMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawExitFasterMany` 메소드는 `withdrawStartMany` 의 txHash를 사용하여 인출 프로세스를 종료하는데 사용할 수 있습니다.

 class="highlight mb-20px mt-20px"> 이는 엔드에서 증명을 생성하기 때문에 빠릅니다. [setProofAPI](/docs/develop/ethereum-polygon/matic-js/set-proof-api) 를 구성해야 합니다. >

**참고** - 인출을 종료하려면 withdrawStart 트랜잭션에 체크포인트가 있어야 합니다.

```
const erc721RootToken = posClient.erc721(<root token address>, true);

const result = await erc721RootToken.withdrawExitFasterMany(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
