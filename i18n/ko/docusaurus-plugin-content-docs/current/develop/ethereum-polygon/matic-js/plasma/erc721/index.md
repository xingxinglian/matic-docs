---
id: index
title: plasmaClient
keywords:
  - 'plasma client, erc721, contract, polygon, sdk'
description: 'maticjs으로 시작하기'
---

# ERC721

`plasmaClient`는 erc721 토큰과 상호 작용하는 데 도움이 되는 `erc721` 메소드를 제공합니다.

이 메소드는 다양한 메소드가 있는 개체를 반환합니다.

```
const erc721token = plasmaClient.erc721(<token address>,<isRoot>);
```

## Child token

이 구문을 사용하여 폴리곤의 토큰을 시작할 수 있습니다 -

```
const childERC20Token = plasmaClient.erc721(<child token address>);
```

## Parent token

이더리움의 토큰은 두 번째 매개변수 값을 `true`로 제공하여 시작할 수 있습니다.

```
const parentERC20Token = plasmaClient.erc721(<parent token address>, true);
```
