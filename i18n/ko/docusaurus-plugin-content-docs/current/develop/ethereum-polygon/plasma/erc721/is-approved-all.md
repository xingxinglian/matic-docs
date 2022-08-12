---
id: is-approved
title: isApprovedAll
keywords:
  - 'plasma client, erc721, isApprovedAll, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# isApprovedAll

`isApprovedAll` 메소드는 모든 토큰이 승인되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc721Token = plasmaClient.erc721(<token address>, true);

const result = await erc721Token.isApprovedAll(<user Address>);

```
