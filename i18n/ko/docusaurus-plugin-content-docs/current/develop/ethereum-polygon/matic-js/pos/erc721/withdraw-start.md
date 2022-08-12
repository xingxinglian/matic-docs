---
id: withdraw-start
title: withdrawStart
keywords:
  - 'pos client, erc721, withdrawStart, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`withdrawStart` 메소드는 폴리곤 체인에서 특정 토큰을 소각할 인출 프로세스를 시작하는 데 사용할 수 있습니다.

```
const erc721Token = posClient.erc721(<child token address>);

const result = await erc721Token.withdrawStart(<token id>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
