---
id: is-approved-all
title: isApprovedAll
keywords:
  - 'pos client, erc1155, isApprovedAll, polygon, sdk'
description: '토큰이 모두 승인되었는지 확인'
---

`isApprovedAll` 메소드는 모든 토큰이 사용자에 대해 승인되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc1155Token = posClient.erc1155(<token address>, true);

const result = await erc1155Token.isApprovedAll(<user Address>);

```
