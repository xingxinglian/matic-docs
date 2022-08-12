---
id: mintable-assets
title: 폴리곤 발행가능 자산
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## 폴리곤 발행가능 토큰이란 무엇인가?

PoS 브리지를 사용하여 이더리움 및 폴리곤 체인 간에 자산을 전송할 수 있습니다. 이러한 자산에는 ERC20, ERC721, ERC1155 및 기타 여러 토큰 표준이 포함됩니다. 자산의 대부분은 이더리움 체인에 이미 존재합니다. 그러나 폴리곤 체인에서도 새로운 자산을 생성하고 필요할 때 이더리움 체인으로 다시 이동할 수 있습니다. 이것은 이더리움에서 토큰 발행에 소요되는 많은 가스와 시간을 절약할 수 있습니다. 폴리곤 체인에서 자산을 생성하는 것이 훨씬 쉽고 권장되는 접근 방식입니다. 이러한 자산은 필요할 때 이더리움 체인으로 이동할 수 있습니다. 이러한 유형의 자산을 폴리곤 발행가능 자산이라고 합니다.

폴리곤 발행가능 토큰의 경우, 자산은 폴리곤에 생성됩니다. 폴리곤 발행 자산을 이더리움으로 이동해야 하는 경우 자산을 먼저 소각한 다음 이 소각 거래의 증명을 이더리움 체인에 제출해야 합니다. RootChainManager 컨트랙트는 내부적으로 특수 술어 컨트랙트를 호출합니다. 이 술어 컨트랙트는 이더리움에서 자산 컨트랙트의 mint 함수를 직접 호출하고 토큰은 사용자 주소로 발행됩니다. 이 특수 술어를 MintableAssetPredicate라고 합니다.

## 충족되어야 하는 요구사항은 무엇입니까?

폴리곤에서 자산을 생성한 다음 이더리움으로 다시 이동해야 할 때 엄격하게 따라야 하는 몇 가지 조건이 있습니다.

### 폴리곤 체인에 배포할 컨트랙트
다음 중 하나를 배포할 수 있습니다.

- 폴리곤 체인의 발행 가능 토큰 컨트랙트 또는
- 매핑 요청을 제출하면 매퍼 도구를 통해 폴리곤 체인에서 발행 가능한 토큰 컨트랙트를 자동으로 배포할 수 있습니다. [https://mapper.polygon.technology/](https://mapper.polygon.technology/)에서 매핑 요청을 제출하고 양식의 하위 컨트랙트 필드를 비워두면 됩니다. 또한 양식에서 Mintable 옵션을 선택하는 것을 잊지 마십시오.

새 매핑 요청을 만드는 방법을 이해하려면 이 [링크](/docs/develop/ethereum-polygon/submit-mapping-request)를 방문하세요.

- 컨트랙트를 직접 배포하려는 경우 하위 컨트랙트는 다음과 같아야 합니다. 이 컨트랙트를 자유롭게 변경할 수 있지만 `deposit`, `withdraw` 및`mint`함수가 있는지 확인하십시오.

    - ChildMintableERC20 -  [https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC20.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC20.sol)
    - ChildMintableERC721 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC721.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC721.sol)
    - ChildMintableERC1155 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC1155.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC1155.sol)

- 가장 중요한 것은 폴리곤에 대한 하위 관리자 컨트랙트에 폴리곤에 배포된 자산 컨트랙트에서 입금자 역할이 부여되어야 한다는 것입니다. 이 하위 관리자 프록시 주소만 폴리곤에 토큰을 입금할 수 있는 권한이 있어야 합니다.
- 매핑 요청을 제출하기 전에 그에 따라 Polygonscan과 Etherscan의 두 컨트랙트를 모두 확인하십시오.

하위 관리자 컨트랙트 주소:

```
뭄바이: 0xb5505a6d998549090530911180f38aC5130101c6
메인넷: 0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa
```

매핑 요청을 제출할 때 배포된 하위 토큰의 컨트랙트 주소를 언급하십시오.

> 이더리움 컨트랙트는 다음 단계에 표시된 대로 배포되어야 합니다 - 하지만 이더리움에서는 발행할 필요가 없습니다. 그것은 필요한 경우 토큰을 이더리움으로 인출할 수 있도록 하기 위해 필요합니다.

### 이더리움에 배포될 컨트랙트

- 토큰 컨트랙트는 이더리움 체인에 배포되어야 하며 다음과 같아야 합니다.
    - MintableERC20 -  [https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC20.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC20.sol)
    - MintableERC721 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC721.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC721.sol)
    - MintableERC1155 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC1155.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC1155.sol)

- 가장 중요한 것은 이더리움에 배포된 `MintableAssetProxy` 컨트랙트에 이더리움에 배포된 자산 컨트랙트에서 발행자 역할을 부여해야 한다는 것입니다. 이 술어 프록시 주소만이 이더리움에서 토큰을 발행할 수 있는 권한을 가져야 합니다.

- 이 역할은 루트 체인의 토큰 컨트랙트에서 grantRole() 함수를 호출하여 부여할 수 있습니다. 첫 번째 매개변수는 **0x12ff340d0cd9c652c747ca35727e68c547d0f0bfa7758d2e77f75acef481b4f2**인 PREDICATE_ROLE 상수 값이고 두 번째 매개변수는 아래에 제공된 토큰 술어 프록시 주소입니다,


    ```jsx
    Ethereum Mainnet
    "MintableERC20PredicateProxy"  : "0x9923263fA127b3d1484cFD649df8f1831c2A74e4",
    "MintableERC721PredicateProxy" : "0x932532aA4c0174b8453839A6E44eE09Cc615F2b7",
    "MintableERC1155PredicateProxy": "0x2d641867411650cd05dB93B59964536b1ED5b1B7
    ```

    ```jsx
    Goerli Testnet
    "MintableERC20PredicateProxy"  : "0x37c3bfC05d5ebF9EBb3FF80ce0bd0133Bf221BC8",
    "MintableERC721PredicateProxy" : "0x56E14C4C1748a818a5564D33cF774c59EB3eDF59",
    "MintableERC1155PredicateProxy": "0x72d6066F486bd0052eefB9114B66ae40e0A6031a",
    ```

