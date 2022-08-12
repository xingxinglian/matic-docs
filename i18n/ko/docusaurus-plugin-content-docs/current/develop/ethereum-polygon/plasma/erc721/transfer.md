---
id: transfer
title: transfer
keywords:
  - 'plasma client, erc721, transfer, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# Transfer

`transfer` 메소드는 한 사용자에서 다른 사용자에게 토큰을 전송합니다.

```
const erc721Token = plasmaClient.erc721(<token address>);

const result = await erc721Token.transfer(<tokenid>,<from>,<to>);

```
