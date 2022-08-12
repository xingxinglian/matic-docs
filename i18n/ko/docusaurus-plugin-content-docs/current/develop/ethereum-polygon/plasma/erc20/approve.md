---
id: approve
title: approve
keywords:
  - 'pos client, erc20, approve, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# approve

`approve` 메소드는 루트 토큰에 필요한 금액을 승인하는 데 사용할 수 있습니다.

폴리곤 체인에 금액을 입금하려면 승인이 필요합니다.

```
const erc20RootToken = plasmaClient.erc20(<root token address>, true);

// approve 100 amount
const approveResult = await erc20Token.approve(100);

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
