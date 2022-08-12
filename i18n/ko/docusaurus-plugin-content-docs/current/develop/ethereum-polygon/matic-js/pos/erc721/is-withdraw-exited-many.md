---
id: is-withdraw-exited-many
title: isWithdrawExitedMany
keywords:
  - 'pos client, erc721, isWithdrawExitedMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`isWithdrawExitedMany` 메소드는 여러 토큰에 대해 인출이 종료되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc721Token = posClient.erc721(<token address>);

const result = await erc721Token.isWithdrawExitedMany(<exit tx hash>);

```
