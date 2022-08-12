---
id: get-allowance
title: getAllowance
keywords:
  - pos client
  - erc20
  - getAllowance
  - polygon
  - sdk
description: getAllowance 메소드를 사용하여 사용자에게 승인된 수량을 얻을 수 있습니다.
---

`getAllowance` 메소드를 사용하여 사용자에게 승인된 수량을 얻을 수 있습니다.

```
const erc20Token = posClient.erc20(<token address>, true);

const balance = await erc20Token.getAllowance(<userAddress>);
```

## spenderAddress

승인된 주소를 `spenderAddress`라고 합니다. 이는 당신을 대신하여 당신의 토큰을 전송할 수 있는 제3자 사용자 또는 스마트 컨트랙트입니다.

기본적으로 spenderAddress 값은 erc20 술어주소입니다.

spenderAddress 값을 수동으로 지정할 수 있습니다.

```
const erc20Token = posClient.erc20(<token address>, true);

const balance = await erc20Token.getAllowance(<userAddress>, {
    spenderAddress: <spender address value>
});
```