---
id: withdraw-exit
title: withdraw exit
keywords:
  - 'plasma client, withdrawExit, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# withdrawExit

플라즈마에서 인출 프로세스는 `withdrawExit` 메소드를 사용하여 누구나 종료할 수 있습니다. 종료 프로세스는 챌린지 기간이 완료된 후에만 작동합니다.

```
const result = plasmaClient.withdrawExit(<token | tokens[]>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

배열에 토큰 목록을 제공하여 여러 토큰에 대해 종료할 수도 있습니다.
