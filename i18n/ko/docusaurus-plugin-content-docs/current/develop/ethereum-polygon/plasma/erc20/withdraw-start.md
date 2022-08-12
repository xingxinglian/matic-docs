---
id: withdraw-start
title: withdraw start
keywords:
  - 'plasma client, erc20, approveMax, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# withdrawStart

`withdrawStart` 은 하위 토큰에서 특정 수량을 소각하는 인출 프로세스를 시작하는 데 사용할 수 있습니다.

```
const erc20ChildToken = plasmaClient.erc20(<child token address>);

// start withdraw process for 100 amount
const result = await erc20ChildToken.withdrawStart(100);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

인출 프로세스에 도전하는 데 사용할 txHash를 저장합니다.
