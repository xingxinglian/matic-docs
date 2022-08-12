---
id: fx-portal
title: Fx-Portal
description: 매핑 없이 이더리움에서 폴리곤으로 상태나 데이터를 전송
keywords:
  - docs
  - matic
  - 폴리곤
image: https://matic.network/banners/matic-network-16x9.png
---

## 개요

폴리곤에서 기본적으로 이더리움 데이터를 읽는 일반적인 메커니즘은 `State Sync`를 사용하는 것입니다.  이를 통해 이더리움에서 폴리곤으로 임의의 데이터를 전송할 수 있습니다. 그러나 이 접근 방식은 기본 인터페이스를 사용할 수 없는 경우 루트 및 하위 컨트랙트의 매핑도 필요합니다. FxPortal은 배포된 기본 FxPortal 컨트랙트를 사용하여 매핑 없이 ERC 표준화 토큰을 배포할 수 있는 대안을 제공합니다.

## [Fx-Portal](https://github.com/fx-portal/contracts)이란 무엇입니까?

강력하면서도 간단한 구현 폴리곤 [상태 동기화](https://docs.polygon.technology/docs/pos/state-sync/state-sync/) 메커니즘입니다. Polygon PoS 브리지는 동일한 아키텍처에 구축됩니다. `examples` 폴더의 코드는 몇 가지 사용 예입니다. 이러한 예제를 사용하여 매핑 없이 모든 상태 동기화를 허용하는 자체 구현 또는 자체 맞춤형 브리지를 쉽게 구축할 수 있습니다.

## 그것은 어떻게 작동합니까?

`FxChild` (FxChild.sol) 및 `FxRoot` (FxRoot.sol)는 FxPortal이 작동하는 주요 컨트랙트입니다. 상태 동기화 메커니즘을 사용하여 매핑 없이 다른 체인의 user-defined 메소드에 데이터를 호출하고 전달합니다. 배포된 기본 컨트랙트를 사용하려면 배포하는 스마트 컨트랙트([FxBaseRootTunnel](https://github.com/fx-portal/contracts/blob/main/contracts/tunnel/FxBaseRootTunnel.sol) 및 [FxBaseChildTunnel](https://github.com/fx-portal/contracts/blob/main/contracts/tunnel/FxBaseChildTunnel.sol))에서 FxPortal의 기본 컨트랙트 구현할 수 있습니다. 이러한 컨트랙트를 기반으로 구축하면 배포된 컨트랙트가 데이터 터널 메커니즘을 사용하여 서로 통신할 수 있습니다.

그렇지 않으면 이미 배포된 터널 컨트랙트와 토큰을 매핑하도록 선택할 수 있습니다.

### ERC20 전송

하위 및 루트 터널 컨트랙트를 통해 루트 체인에서 토큰을 입금하고 하위 체인에서 출금할 수 있습니다.

#### `FxERC20RootTunnel`

- `mapToken(address rootToken)` 배포된 컨트랙트에서 함수를 호출하여 ERC20 토큰을 매핑하고 하위 체인에 해당하는 하위 토큰을 생성할 수 있습니다.
- `deposit(address rootToken, address user, uint256 amount, bytes memory data)` 매핑된 토큰의 주소, 해당 수량으로 인출할 수 있는 주소(필요한 경우 데이터와 함께)로 deposit()을 호출합니다. 토큰을 먼저 사용하려면 표준 ERC20 `approve` 함수를 사용하여 컨트랙트를 승인해야 합니다.

#### `FxERC20ChildTunnel`

- `withdraw(address childToken, uint256 amount)` deposit()에 할당된 주소는 이 함수를 사용하여 하위 토큰의 모든 금액을 인출할 수 있습니다. 그들은 처음 매핑될 때 생성된 하위 토큰을 받습니다.
- `rootToChildToken` 이 공용 변수에는 루트 토큰과 하위 토큰 매핑이 포함됩니다. 루트 토큰의 주소로 매핑을 쿼리하여 배포된 하위 토큰의 주소를 알 수 있습니다.

#### 이더리움에서 폴리곤으로의 ERC20 전송을 위한 단계

1. 루트 체인에 자체 ERC20 토큰을 배포합니다. 나중에 이 주소가 필요합니다.
2. 루트 터널 주소와 수량을 인수로 하여 루트 토큰의 `approve()`함수를 호출하여 토큰 전송을 승인합니다.
3. 루트 체인의 수신자 주소와 수량으로 `deposit()` 호출을 진행하여 하위 체인에서 동등한 하위 토큰을 받습니다. 이렇게 하면 토큰도 자동으로 매핑됩니다. 또는 입금하기 전에 먼저 `mapToken()`을 호출할 수 있습니다.
4. 그게 다입니다! 🎉 매핑이 끝나면 이제 터널의 `deposit` 및 `withdraw` 함수를 사용하여 교차 체인 송금을 실행할 수 있습니다.

**참고**: 루트 체인에서 `deposit()`을 수행한 후 상태 동기화가 발생하는 데 10-15분이 걸립니다. 상태 동기화가 발생하면 지정된 주소에 토큰이 입금됩니다.

### 폴리곤에서 이더리움으로의 ERC20 전송을 위한 단계

1. 1.	하위 컨트랙트의 인수로 해당 토큰 주소와 금액을 사용하여 `withdraw()`를 호출하여 하위 토큰을 루트 체인의 지정된 수신자로 다시 이동합니다. 소각 증명을 생성하는 데 사용되므로 **tx 해시에 유의하십시오**.

### 이더리움에서 폴리곤으로의 ERC721 전송을 위한 단계

1. 루트 체인에 고유한 ERC721 토큰을 배포합니다. 나중에 이 주소가 필요합니다.
2. 루트 터널 주소와 토큰 ID를 인수로 사용하여 루트 토큰의 `approve()`함수를 호출하여 토큰 전송을 승인합니다.
3. 루트 체인의 수신자 주소와 토큰 ID로 `deposit()`을 호출하여 하위 체인에서 동등한 하위 토큰을 받습니다. 이렇게 하면 토큰도 자동으로 매핑됩니다. 또는 입금하기 전에 먼저 `mapToken()`을 호출할 수 있습니다.

**참고**: 루트 체인에서 `deposit()`을 수행한 후 상태 동기화가 발생하는 데 10-15분이 걸립니다. 상태 동기화가 발생하면 지정된 주소에 토큰이 입금됩니다.

#### 폴리곤에서 이더리움으로의 ERC721전송을 위한 단계

1. 하위 컨트랙트의 인수로 해당 토큰 주소와 토큰 ID를 사용하여 `withdraw()`를 호출하여 하위 토큰을 루트 체인의 지정된 수신자로 다시 이동합니다. 소각 증명을 생성하는 데 사용되므로 **tx 해시에 유의하십시오**.

### ERC1155 전송

#### `FxERC1155RootTunnel`

- `mapToken(rootToken)`: 루트 ERC1155 토큰을 하위 체인에 매핑하는 데 사용됩니다.
- `deposit(rootToken, user, id, amount, data)`: 루트 토큰을 하위 체인에 입금하는 데 사용되는 함수
- `depositBatch(rootToken, user, ids, amounts, bytes memory data)`: 여러 토큰 ID 및 해당 수량에 사용
- •`receiveMessage(inputData)`: 페이로드를 `inputData`로 사용하여 소각 증명이 생성된 후 호출됩니다.

#### `FxERC1155ChildTunnel`

- `withdraw(childToken, id, amount, data)`: 폴리곤에서 이더리움으로 토큰을 인출할 때 사용
- `withdrawBatch(childToken, ids, amounts, data)`: 인출과 동일하지만 여러 토큰 ID를 인출하는 경우

#### 이더리움에서 폴리곤으로의 ERC1155토큰 입금을 위한 단계

1. 루트 체인에 ERC1155 토큰을 배포합니다. 나중에 이 주소가 필요합니다.
2. FxERC1155RootTunnel이 토큰을 폴리곤의 FxERC1155ChildTunnel로 전송할 수 있도록 FxERC1155RootTunnel의 주소를 `operator`로 사용하여 배포된 토큰에서 `setApprovalForAll(operator, approved)`을 호출합니다.
3. 배포된 토큰의 주소를 `rootToken`으로 사용하여 FxERC1155RootTunnel에서 `mapToken()`을 호출합니다. 이렇게 하면 FxERC1155ChildTunnel에 메시지를 보내서 폴리곤에 ERC1155 토큰을 배포하고 매핑하도록 지시합니다. 하위 토큰 주소를 쿼리하려면 FxERC1155ChildTunnel에서 `rootToChildToken`을 호출하십시오.
4. FxERC1155RootTunnel에서 이더리움의 토큰 주소를 `rootToken`으로, 수신자를 `user`로, 토큰 ID를 `id`로, 수량을 `amount`으로 사용하여 `deposit()`을 호출합니다. 또는 여러 토큰 ID에 대해 `depositBatch()`를 호출할 수도 있습니다.

**참고**: 루트 체인에서 `deposit()`을 수행한 후 상태 동기화가 발생하는 데 10-15분이 걸립니다. 상태 동기화가 발생하면 지정된 주소에 토큰이 입금됩니다.

#### 폴리곤에서 이더리움으로의 ERC1155를 인출하는 단계

1. 폴리곤에 배포된 하위 토큰의 주소를 `childToken`으로, 토큰 ID를 `id`로 사용하여 FxERC1155ChildTunnel에서 `withdraw()`를 호출합니다(하위 토큰 주소는 `rootToChildToken` 매핑에서 쿼리할 수 있음). 또는 여러 토큰 ID 및 해당 수량에 대해 `withdrawBatch()`를 호출할 수도 있습니다. 소각 증명을 생성하는 데 사용되므로 **tx 해시에 유의하십시오**.

### 루트체인에서 토큰 인출하기

**참고**: 자식 체인에서 `withdraw()`를 수행한 후 체크포인트가 발생하는 데 30-90분이 걸립니다. 다음 체크포인트에 소각 tx가 포함되면 루트 체인에서 토큰을 인출할 수 있습니다.

1. tx 해시 및 MESSAGE_SENT_EVENT_SIG를 사용하여 소각 증명을 생성합니다. 증명을 생성하는 예제 스크립트는 [여기](https://gist.github.com/QEDK/62c4503d9a6a4bc57c491ee09376d71a)에서 찾을 수 있습니다.
2. 생성된 페이로드를 해당 루트 터널 컨트랙트의 `receiveMessage()`에 대한 인수로 제공합니다.

### Mintable ERC-20 전송하기

#### `FxMintableERC20RootTunnel`

- `deposit(address rootToken, address user, uint256 amount, bytes memory data)`: 이더리움에서 폴리곤으로 토큰을 입금
- `receiveMessage(bytes memory inputData)`: 루트 체인에서 토큰을 받기 위해 `inputData`로 제공할 소각 증명.

#### `FxMintableERC20ChildTunnel`

- •`deployChildToken(uint256 uniqueId, string memory name, string memory symbol, uint8 decimals)`: 폴리곤 체인에 ERC20 토큰을 배포
- `mintToken(address childToken, uint256 amount)`: 폴리곤에 특정 수량의 토큰을 발행.
- `withdraw(address childToken, uint256 amount)`: 루트 체인에서 인출하기 위해 하위 체인에서 토큰을 소각.

#### 폴리곤에서 토큰을 발행하는 단계

1. `FxMintableERC20ChildTunnel`에서 `deployChildToken()`을 호출하고 필요한 토큰 정보를 매개변수로 전달합니다. 이것은 `rootToken` 및 `childToken` 주소를 포함하는 `TokenMapped` 이벤트를 내보냅니다. 이 주소를 기록해 두십시오.
2. `FxMintableERC20ChildTunnel`에서 `mintToken()`을 호출하여 하위 체인에서 토큰을 발행합니다.
3. `FxMintableERC20ChildTunnel`에서 `withdraw()`를 호출하여 폴리곤에서 토큰을 출금합니다. 소각 증명을 생성하는 데 유용하므로 tx 해시를 기록해 두십시오.
4. 소각 tx가 체크포인트에 포함될 때까지 기다립니다(~30-45분). 그런 다음 [여기](https://gist.github.com/QEDK/62c4503d9a6a4bc57c491ee09376d71a)에서 예제 스크립트를 사용하여 소각 증명을 생성합니다.

#### 이더리움에서 토큰 인출을 위한 단계

`FxMintableERC20RootTunnel`의 `receiveMessage()`에 대한 인수로 생성된 소각 증명을 제공합니다. 그 후, 토큰 잔고는 루트 체인에 반영됩니다.

#### 이더리움에서 폴리곤으로 토큰을 다시 입금하는 단계

1. 토큰을 전송하려면 `FxMintableERC20RootTunnel`을 승인해야 합니다.
2. `FxMintableERC20RootTunnel`에서 `rootToken` 루트 토큰의 주소로, `user`를 수신자로 사용하여 `deposit()`을 호출합니다.
3. 상태 동기화 이벤트를 기다립니다(~10-15분). 그런 다음 하위 체인에서 대상 수신자의 잔고를 쿼리할 수 있습니다.

## 배포 예시

Goerli:

- Checkpoint Manager: 0x2890bA17EfE978480615e330ecB65333b880928e
- Dummy ERC20 token: 0xe9c7873f81c815d64c71c2233462cb175e4765b3
- FxERC20RootTunnel: 0x3658ccFDE5e9629b0805EB06AaCFc42416850961
- FxMintableERC20RootTunnel: 0xA200766a7D64E54611E2D232AA6c1f870aCb63c1
- Dummy ERC721 token: 0x73594a053cb5ddDE5558268d28a774375C4E23dA
- FxERC721RootTunnel: 0xF9bc4a80464E48369303196645e876c8C7D972de
- Dummy ERC1155 Token: 0x1906d395752FE0c930f8d061DFEb785eBE6f0B4E
- FxERC1155RootTunnel : 0x48DE785970ca6eD289315036B6d187888cF9Df48

뭄바이:

- FxERC20: 0xDDE69724AeFBdb084413719fE745aB66e3b055C7
- FxERC20ChildTunnel: 0x9C37aEbdb7Dd337E0215BC40152d6689DaF9c767
- FxMintableERC20ChildTunnel: 0xA2C7eBEf68B444056b4A39C2CEC23844275C56e9
- Child token dummy ERC20: 0x346d21bc2bD3dEE2d1168E1A632b10D1d7B9c0A
- FxERC721: 0xf2720927E048726267C0221ffA41A88528048726
- FxERC721ChildTunnel: 0x3658ccFDE5e9629b0805EB06AaCFc42416850961
- FxERC1155: 0x80be8Cf927047A40d3f5791BF7436D8c95b3Ae5C
- FxERC1155ChildTunnel: 0x3A0f90D3905601501652fe925e96d8B294243Efc

## 컨트랙트 주소

**뭄바이**

| 컨트랙트                                                                                                         | 배포된 주소                                       |
|:------------------------------------------------------------------------------------------------------------ |:-------------------------------------------- |
| [FxRoot (Goerli)](https://goerli.etherscan.io/address/0x3d1d3E34f7fB6D26245E6640E1c50710eFFf15bA#code)       | `0x3d1d3E34f7fB6D26245E6640E1c50710eFFf15bA` |
| [FxChild (뭄바이)](https://mumbai.polygonscan.com/address/0xCf73231F28B7331BBe3124B907840A94851f9f11/contracts) | `0xCf73231F28B7331BBe3124B907840A94851f9f11` |

**메인넷**


| 컨트랙트                                                                                                      | 배포된 주소                                       |
|:--------------------------------------------------------------------------------------------------------- |:-------------------------------------------- |
| [FxRoot (이더리움 메인넷)](https://etherscan.io/address/0xfe5e5d361b2ad62c541bab87c45a0b9b018389a2#code)         | `0xfe5e5D361b2ad62c541bAb87C45a0B9B018389a2` |
| [FxChild (폴리곤 메인넷)](https://polygonscan.com/address/0x8397259c983751DAf40400790063935a11afa28a/contracts) | `0x8397259c983751DAf40400790063935a11afa28a` |
