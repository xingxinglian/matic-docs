---
id: is-aproved
title: isApproved
keywords:
  - 'plasma client, erc721, isApproved, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`isApproved` 메소드는 지정된 tokenId에 대해 토큰이 승인되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc721Token = plasmaClient.erc721(<token address>, true);

const result = await erc721Token.isApproved(<tokenId>);

```
