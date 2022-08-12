---
id: is-withdraw-exited
title: isWithdrawExited
keywords:
  - 'pos client, erc20, isWithdrawExited, polygon, sdk'
description: 'isWithdrawExited 메소드를 사용하여 인출이 종료되었는지 여부를 알 수 있습니다.'
---

`isWithdrawExited` 메소드를 사용하여 인출이 종료되었는지 여부를 알 수 있습니다.

```
const erc20RootToken = posClient.erc20(<root token address>,true);

const isExited = await erc20Token.isWithdrawExited(<burn tx hash>);
```
