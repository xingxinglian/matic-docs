---
id: is-withdraw-exited-many
title: isWithdrawExitedMany
keywords:
  - 'pos client, erc1155, isWithdrawExitedMany, polygon, sdk'
description: 'isWithdrawExitedMany 메소드는 여러 토큰에 대해 인출이 종료되었는지 확인합니다.'
---

`isWithdrawExitedMany` 메소드는 여러 토큰에 대해 인출이 종료되었는지 확인합니다.  부울 값을 반환합니다.

```
const erc1155Token = posClient.erc1155(<token address>);

const result = await erc1155Token.isWithdrawExitedMany(<exit tx hash>);

```
