---
id: is-withdraw-exited
title: isWithdrawExited
keywords:
  - 'pos client, erc721, isWithdrawExited, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`isWithdrawExited` 메소드는 인출이 종료되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc721Token = posClient.erc721(<token address>);

const result = await erc721Token.isWithdrawExited(<exit tx hash>);

```
