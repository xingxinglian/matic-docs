---
id: withdraw-confirm-faster
title: withdrawChallengeFaster
keywords:
  - 'plasma client, erc721, withdrawChallengeFaster, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawConfirmFaster` 메소드는 플라즈마 인출 프로세스의 두 번째 단계입니다. 이 단계에서는 소각 거래(첫 번째 트랜잭션)의 증명이 제출되고 동등한 가치의 erc721 토큰이 생성됩니다.

이 과정이 성공하면 챌린지 기간이 시작되고 챌린지 기간이 완료되면 사용자는 루트 체인에 있는 자신의 계정으로 출금된 금액을 돌려받을 수 있습니다.

챌린지 기간은 메인넷의 경우 7일입니다.

 class="highlight mb-20px mt-20px"> 이는 백엔드에서 증명을 생성하기 때문에 빠릅니다.  [setProofAPI](/docs/develop/ethereum-polygon/matic-js/set-proof-api)를 구성해야 합니다.

```
const erc721Token = plasmaClient.erc721(<token address>, true);

const result = await erc721Token.withdrawConfirmFaster(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
