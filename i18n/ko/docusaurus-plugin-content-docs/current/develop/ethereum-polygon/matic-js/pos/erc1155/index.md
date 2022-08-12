---
id: index
title: POSClient
keywords:
  - 'pos client, erc1155, contract, polygon, sdk'
description: 'matic.js를 이용한 erc1155토큰과 상호작용'
---

# ERC1155

<code>POSClient</code>는 erc1155 토큰과 상호 작용하는 데 도움이 되는 `erc1155` 메서드를 제공합니다.

메소드는 다른 메소드를 포함하는 **ERC1155** 클래스의 인스턴스를 반환합니다.

```
const erc721token = posClient.erc1155(<token address>, <isRoot>);
```

`isRoot`에 대한 두 번째 인수 전달은 선택 사항입니다.

## Child token(하위 토큰)

이 구문을 사용하여 폴리곤의 토큰을 시작할 수 있습니다 -

```
const childERC20Token = posClient.erc1155(<child token address>);
```

## Parent token(상위 토큰)

이더리움의 토큰은 두 번째 매개변수 값을 `true`로 제공하여 시작할 수 있습니다.

```
const parentERC20Token = posClient.erc1155(<parent token address>, true);
```
