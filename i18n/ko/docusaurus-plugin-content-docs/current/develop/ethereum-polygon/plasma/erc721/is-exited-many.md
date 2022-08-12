---
id: is-exited-many
title: isExitedMany
keywords:
  - 'plasma client, erc721, isExitedMany, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# isExitedMany

`isExitedMany` 메소드는 인출이 종료되었는지 확인합니다. 부울 값을 반환합니다.

```
const erc721Token = plasmaClient.erc721(<token address>);

const result = await erc721Token.isExitedMany(<exit tx hash>);

```
