---
id: get-balance
title: getBalance
keywords:
  - 'pos client, erc1155, getBalance, polygon, sdk'
description: 'matic.js를 이용한 erc1155토큰의 잔고 얻기'
---

`getBalance` 메소드를 사용하여 토큰에 대한 사용자의 잔고를 얻을 수 있습니다. 하위 토큰과 상위 토큰 모두에서 사용할 수 있습니다.

```
const erc1155Token = posClient.erc1155(<token address>);

// get balance of user
const balance = await erc1155Token.getBalance(<userAddress>, <token id>);
```
