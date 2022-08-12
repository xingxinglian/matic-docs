---
id: deposit-many
title: deplasmaitMany
keywords:
  - 'plasma client, erc721, deplasmaitMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# deplasmaitMany

`deplasmaitMany` 메소드는 이더리움에서 폴리곤 체인으로 여러 토큰을 디플라스마하는 데 사용할 수 있습니다.

```
const erc721RootToken = plasmaClient.erc721(<root token address>, true);

const result = await erc721RootToken.deplasmaitMany([<token id1>,<token id2>], <user address>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
