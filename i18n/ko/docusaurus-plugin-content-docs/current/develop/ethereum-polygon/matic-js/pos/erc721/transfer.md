---
id: transfer
title: transfer
keywords:
  - 'pos client, erc721, transfer, polygon, sdk'
description: '한 사용자에서 다른 사용자에게 토큰을 전송'
---

`transfer` 메소드는 한 사용자에서 다른 사용자에게 토큰을 전송하는 데 사용할 수 있습니다.

```
const erc721Token = posClient.erc721(<token address>);

const result = await erc721Token.transfer(<tokenid>,<from>,<to>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
