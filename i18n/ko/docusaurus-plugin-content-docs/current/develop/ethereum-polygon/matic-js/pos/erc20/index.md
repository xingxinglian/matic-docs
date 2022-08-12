---
id: index
title: POSClient
keywords:
  - pos client
  - erc20
  - 컨트랙트
  - 폴리곤
  - sdk
description: POSClient는 ERC20 토큰과 상호작용할 수 있도록 도와주는 erc20 메소드를 제공합니다.
---

# ERC20

`POSClient`는 **ERC20** 토큰과 상호작용할 수 있도록 도와주는 `erc20` 메소드를 제공합니다.

이 메소드는 다른 다양한 메소드가 있는 개체를 반환합니다.

```
const erc20token = posClient.erc20(<token address>,<isRoot>);
```

`isRoot`에 대한 두 번째 인수 전달은 선택 사항입니다.

## Child token(하위 토큰)

이 구문을 사용하여 폴리곤의 토큰을 시작할 수 있습니다 -

```
const childERC20Token = posClient.erc20(<child token address>);
```

## Parent token(상위 토큰)

이더리움의 토큰은 두 번째 매개변수 값을 `true`로 제공하여 시작할 수 있습니다.

```
const parentERC20Token = posClient.erc20(<parent token address>, true);
```
