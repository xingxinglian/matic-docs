---
id: withdraw-start-many
title: withdrawStartMany
keywords:
  - 'pos client, erc721, withdrawStartMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawStartMany` 메소드는 폴리곤 체인에 여러 토큰을 소각할 인출 프로세스를 시작하는 데 사용할 수 있습니다.

```
const erc721Token = posClient.erc721(<child token address>);

const result = await erc721Token.withdrawStartMany([<token id1>, <token id2>]);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
