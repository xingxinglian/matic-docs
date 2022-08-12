---
id: get-all-tokens
title: getAllTokens
keywords:
  - 'pos client, erc721, getAllTokens, polygon, sdk'
description: 'maticjs으로 시작하기'
---

`getAllTokens` 메소드는 지정된 사용자가 소유한 모든 토큰을 반환합니다.

```
const erc721Token = posClient.erc721(<token address>);

const result = await erc721Token.getAllTokens(<user address>, <limit>);

```

두 번째 매개변수에 제한 값을 지정하여 토큰을 제한할 수도 있습니다.
