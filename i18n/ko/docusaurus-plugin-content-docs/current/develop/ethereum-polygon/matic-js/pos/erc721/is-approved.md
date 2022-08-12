---
id: is-approved
title: isApproved
keywords:
  - 'pos client, erc721, isApproved, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`isApproved` 메소드는 특정 tokenId에 대해 토큰이 승인되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc721Token = posClient.erc721(<token address>, true);

const result = await erc721Token.isApproved(<tokenId>);

```
