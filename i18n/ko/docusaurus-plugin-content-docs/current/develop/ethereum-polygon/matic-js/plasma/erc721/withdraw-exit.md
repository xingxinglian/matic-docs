---
id: withdraw-exit
title: withdraw exit
keywords:
  - 'pos client, erc721, withdrawExit, polygon, sdk'
description: 'maticjs으로 시작하기'
---

챌린지 기간이 완료되면 인출 프로세스를 종료하는 데 `withdrawExit` 메소드를 사용할 수 있습니다.

```
const erc20RootToken = plasmaClient.erc721(<root token address>, true);

const result = await erc20Token.withdrawExit();

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
