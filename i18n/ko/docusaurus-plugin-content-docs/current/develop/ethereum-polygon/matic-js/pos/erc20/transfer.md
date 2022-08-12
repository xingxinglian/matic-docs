---
id: transfer
title: transfer
keywords:
  - 'POS client, erc20, transfer, polygon, sdk'
description: 'transfer 메소드는 한 주소에서 다른 주소로 금액을 이체하는 데 사용할 수 있습니다.'
---

`transfer` 메소드는 한 주소에서 다른 주소로 금액을 이체하는 데 사용할 수 있습니다.

```
const erc20Token = posClient.erc20(<token address>);

const result = await erc20Token.transfer(<amount>,<to>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
