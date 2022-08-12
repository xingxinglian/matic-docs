---
id: deposit
title: deposit
keywords:
  - 'pos client, erc1155, deposit, polygon, sdk'
description: 'matic.js를 이용하여 erc1155토큰 입금'
---

`deposit` 메소드는 이더리움에서 폴리곤 체인에 필요한 수량의 토큰을 입금하는 데 사용할 수 있습니다.

```
onst erc1155RootToken = posClient.erc1155(<root token address>, true);

const result = await erc1155RootToken.deposit({
    amount: 1,
    tokenId: '123',
    userAddress: <from address>,
    data: '0x5465737445524331313535', // data is optional
});

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

**데이터** 제공은 선택 사항입니다.