---
id: transfer
title: transfer
keywords:
  - 'plasma client, erc20, transfer, polygon, sdk'
description: 'erc20 플라즈마 토큰 전송'
---

`transfer` 메소드는 한 주소에서 다른 주소로 금액을 이체하는 데 사용할 수 있습니다.

```
const erc20Token = plasmaClient.erc20(<token address>);

const result = await erc20Token.transfer(<amount>,<to>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

## MATIC 토큰 전송

MATIC은 폴리곤의 기본 토큰입니다. 따라서 토큰 주소 없이 matic 토큰의 전송을 지원합니다.

```
// initialize token with null means use MATIC tokens
const erc20Token = plasmaClient.erc20(null);

const result = await erc20Token.transfer(<amount>,<to>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();
```
