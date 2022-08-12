---
id: approve-max
title: approveMax
keywords:
  - pos client
  - erc20
  - approveMax
  - polygon
  - sdk
description: 'acceptMax 메소드를 사용하여 루트 토큰의 최대 금액을 승인할 수 있습니다.'
---

`approveMax` 메소드는 루트 토큰의 최대 금액을 승인하는 데 사용할 수 있습니다.

```
const erc20RootToken = posClient.erc20(<root token address>, true);

const approveResult = await erc20RootToken.approveMax();

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```

## spenderAddress

승인된 주소를 `spenderAddress`라고 합니다. 이는 당신을 대신하여 당신의 토큰을 전송할 수 있는 제3자 사용자 또는 스마트 컨트랙트입니다.

기본적으로 spenderAddress 값은 erc20 술어주소입니다.

spenderAddress 값을 수동으로 지정할 수 있습니다.

```
const erc20RootToken = posClient.erc20(<root token address>,true);

// approve 100 amount
const approveResult = await erc20Token.approveMax({
    spenderAddress: <spender address value>
});

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
