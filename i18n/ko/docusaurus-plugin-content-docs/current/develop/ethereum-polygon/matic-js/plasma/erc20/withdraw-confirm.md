---
id: withdraw-confirm
title: withdraw challenge
keywords:
  - 'plasma client, erc20, withdrawChallenge, polygon, sdk'
description: 'maticjs로 시작하기'
---

`withdrawConfirm` 은 플라즈마 인출 프로세스의 두 번째 단계입니다.  이 단계에서는 소각 거래(첫 번째 트랜잭션)의 증명이 제출되고 동등한 가치의 erc721 토큰이 생성됩니다.

이 프로세스가 성공한 후 - 챌린지 기간이 시작되고 챌린지 기간이 완료되면 사용자는 출금된 수량을 루트 체인의 계정으로 되돌릴 수 있습니다.

챌린지 기간은 메인넷의 경우 7일입니다.

**참고** - 인출을 시도하려면 withdrawStart 트랜잭션에 체크포인트가 있어야 합니다.

```
const erc20Token = plasmaClient.erc20(<token address>, true);

const result = await erc20Token.withdrawConfirm(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

챌린지 기간이 완료되면 인출 프로세스를 종료하고 인출된 금액을 반환하기 위해 `withdrawExit`를 호출할 수 있습니다.
