---
id: withdraw-start
title: withdrawStart
keywords:
  - 'pos client, erc1155, withdrawStart, polygon, sdk'
description: '폴리곤 체인에서 토큰 ID의 특정 수량을 소각할 인출 프로세스를 시작'
---

`withdrawStart` 메소드는 폴리곤 체인에서 토큰 ID의 특정 수량을 소각할 인출 프로세스를 시작하는 데 사용할 수 있습니다.

```
const erc1155Token = posClient.erc1155(<child token address>);

const result = await erc1155Token.withdrawStart(<token id>,<amount>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
