---
id: get-allowance
title: getAllowance
keywords:
  - 'plasma client, erc20, getAllowance, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# getAllowance

`getAllowance` 메소드를 사용하여 사용자에게 승인된 수량을 얻을 수 있습니다.

```
const erc20Token = plasmaClient.erc20(<token address>, true);

const balance = await erc20Token.getAllowance(<userAddress>);
```
