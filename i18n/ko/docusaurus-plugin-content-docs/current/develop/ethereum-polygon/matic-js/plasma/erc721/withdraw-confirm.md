---
id: withdraw-confirm
title: withdrawChallenge
keywords:
  - 'plasma client, erc721, withdrawChallenge, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawConfirm` 메소드는 플라즈마 인출 프로세스의 두 번째 단계입니다.  이 단계에서는 소각 거래(첫 번째 트랜잭션)의 증명이 제출되고 동등한 가치의 erc721 토큰이 생성됩니다.

이 과정이 성공하면 챌린지 기간이 시작되고 챌린지 기간이 완료되면 사용자는 루트 체인에 있는 자신의 계정으로 출금된 금액을 돌려받을 수 있습니다.

챌린지 기간은 메인넷의 경우 7일입니다.

```
const erc721Token = plasmaClient.erc721(<token address>, true);

const result = await erc721Token.withdrawConfirm(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
