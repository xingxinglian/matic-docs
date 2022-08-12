---
id: get-balance
title: getBalance
keywords:
  - 'pos client, erc20, getBalance, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# getBalance

`getBalance` 메소드를 사용하여 사용자의 잔고를 얻을 수 있습니다.  하위 토큰과 상위 토큰 모두에서 사용할 수 있습니다.

```
const erc20Token = plasmaClient.erc20(<token address>);

// get balance of user
const balance = await erc20Token.getBalance(<userAddress>);
```
