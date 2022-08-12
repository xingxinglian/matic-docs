---
id: index
title: PlasmaClient
keywords:
  - 'plasma client, erc20, contract, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`plasmaClient`는 `erc20` 토큰과 상호작용할 수 있도록 도와주는 erc20 메소드를 제공합니다.

## 하위 토큰

```
const childERC20Token = plasmaClient.erc20(<child token address>);
```

## 루트 토큰

루트 토큰은 두 번째 매개변수 값을 `true`로 제공하여 시작할 수 있습니다.

```
const parentERC20Token = plasmaClient.erc20(<root token address>, true);
```
