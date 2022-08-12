---
id: transfer
title: transfer
keywords:
  - 'pos client, erc1155, transfer, polygon, sdk'
description: '한 사용자에서 다른 사용자에게 토큰을 전송'
---

`transfer` 메소드는 한 사용자에서 다른 사용자에게 토큰을 전송하는 데 사용할 수 있습니다.

```
const erc1155Token = posClient.erc1155(<token address>);

const result = await erc1155Token.transfer({
    tokenId: <tokenId>,
    amount: <amount>,
    from : <from address>,
    to : <to address>,
    data : <data to sent>, // data is optional
});

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
