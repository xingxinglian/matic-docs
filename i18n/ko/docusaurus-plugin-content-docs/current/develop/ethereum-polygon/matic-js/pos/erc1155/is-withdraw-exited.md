---
id: is-withdraw-exited
title: isWithdrawExited
keywords:
  - 'pos client, erc1155, isWithdrawExited, polygon, sdk'
description: 'isWithdrawExited 메소드는 인출이 종료되었는지 확인합니다.'
---

`isWithdrawExited` 메소드는 인출이 종료되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc1155Token = posClient.erc1155(<token address>);

const result = await erc1155Token.isWithdrawExited(<exit tx hash>);

```
