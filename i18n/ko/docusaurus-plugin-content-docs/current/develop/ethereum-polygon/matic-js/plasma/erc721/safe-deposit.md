---
id: safe-deposit
title: safeDeposit
keywords:
  - 'plasma client, erc721, deplasmait, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`safeDeposit` 메소드는 이더리움에서 폴리곤 체인으로 토큰을 입금하는 데 사용할 수 있습니다.

```
const erc721RootToken = plasmaClient.erc721(<root token address>, true);

const result = await erc721RootToken.safeDeposit(<token id>, <user address>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
