---
id: withdraw-exit-faster
title: withdraw exit faster
keywords:
  - 'pos client, erc20, withdrawExitFaster, polygon, sdk'
description: 'withdrawExitFaster 메소드는 withdrawStart 메소드의 txHash를 사용하여 인출 프로세스를 더 빨리 종료하는 데 사용할 수 있습니다.'
---

`withdrawExitFaster` 메소드는 `withdrawStart` 메소드의 txHash를 사용하여 인출 프로세스를 더 빨리 종료하는 데 사용할 수 있습니다.

백엔드에서 증명을 생성하기 때문에 일반적으로 빠릅니다. 당신은 [setProofAPI](/docs/develop/ethereum-polygon/matic-js/set-proof-api)를 구성해야 합니다.

**참고** - 인출을 종료하려면 withdrawStart 트랜잭션에 체크포인트가 있어야 합니다.

```
import { setProofApi } from '@maticnetwork/maticjs'

setProofApi("https://apis.matic.network/");

const erc20RootToken = posClient.erc20(<root token address>, true);

// start withdraw process for 100 amount
const result = await erc20Token.withdrawExitFaster(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

트랜잭션이 완료되고 체크포인트가 완료되면 금액이 루트 체인에 입금됩니다.
