---
id: withdraw-exit-faster
title: withdrawExitFaster
keywords:
  - 'pos client, erc1155, withdrawExitFaster, polygon, sdk'
description: 'withdrawExitFaster 메소드는 withdrawStart의 txHash를 사용하여 인출 프로세스를 종료하는 데 사용할 수 있습니다.'
---

`withdrawExitFaster` 메소드는 `withdrawStart`의 txHash를 사용하여 인출 프로세스를 종료하는 데 사용할 수 있습니다.

 class="highlight mb-20px mt-20px"> 이는 백엔드에서 증명을 생성하기 때문에 빠릅니다.  [setProofAPI](/docs/develop/ethereum-polygon/matic-js/set-proof-api)를 구성해야 합니다.

**참고** - 인출을 종료하려면 withdrawStart 트랜잭션에 체크포인트가 있어야 합니다.

```
const erc1155RootToken = posClient.erc1155(<root token address>, true);

const result = await erc1155RootToken.withdrawExitFaster(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
