---
id: get-token-id-at-index-for-user
title: getTokenIdAtIndexForUser
keywords:
  - 'pos client, erc721, getTokenIdAtIndexForUser, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`getTokenIdAtIndexForUser` 메소드는 사용자에 대해 제공된 인덱스의 토큰 ID를 반환합니다.

```
const erc721Token = posClient.erc721(<token address>);

const result = await erc721Token.getTokenIdAtIndexForUser(<index>,<user address>);

```
