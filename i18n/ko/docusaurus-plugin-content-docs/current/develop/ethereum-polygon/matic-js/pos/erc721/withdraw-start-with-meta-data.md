---
id: withdraw-start-with-meta-data
title: withdrawStartWithMetaData
keywords:
  - 'pos client, erc721, withdrawStartWithMetaData, polygon, sdk'
description: 'withdrawStartWithMetaData 메소드는 인출 프로세스를 시작하는데 사용될 수 있습니다.'
---

`withdrawStartWithMetaData` 메소드는 폴리곤 체인에서 특정 토큰을 소각할 인출 프로세스를 시작하는 데 사용할 수 있습니다.  후드 아래에서는 토큰 컨트랙트에서 `withdrawWithMetadata` 메소드를 호출합니다.


```
const erc721Token = posClient.erc721(<token address>);

const result = await erc721Token.withdrawStartWithMetaData(<token id>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
