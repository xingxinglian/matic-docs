---
id: swap-assets
title: 자산 스왑하기
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## 플라즈마 자산 스왑을 사용하여 ERC20 및 ERC721 토큰을 원자적으로 스왑하기

이 문서는 폴리곤을 사용하는 동안 수행할 수 있는 Plasma 자산 스왑을 이해하는 데 도움이 됩니다. 이를 통해 이더리움의 보안에 편승하는 Plasma 구성을 사용하는 동안 탈중앙화 거래소, NFT 마켓플레이스 등과 같은 응용 프로그램을 만들 수 있습니다.

## EIP712 및 서명된 전송에 대한 소개
이 섹션은 폴리곤 플라즈마 체인에서 매핑된 자산의 스왑에 대한 소개를 제공하는 것을 목표로 합니다.
> 참고: 폴리곤에 직접 배포된 토큰의 경우 프로세스가 필요하지 않습니다. 프로세스는 폴리곤에 *매핑된* 토큰에만 적용됩니다.

전송 프로세스는 [EIP712](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md)에 도입된 새로운 RPC 호출 `eth_SignTypedData`를 사용하여 활성화됩니다. 이는 플라즈마 체인에 대한 허용의 복잡성을 피하고 플라즈마 사기 검증에 단순성을 추가하기 위해 수행됩니다.

이 구축에는 `transferWithSig`라고 하는 폴리곤 플라즈마 체인의 [ERC721](https://github.com/maticnetwork/contracts/blob/aee2433b2cb76b8bf2ad53736a9e6340cd3d9f15/contracts/child/ChildERC721.sol#L76) 및 [ERC20](https://github.com/maticnetwork/contracts/blob/aee2433b2cb76b8bf2ad53736a9e6340cd3d9f15/contracts/child/ChildERC20.sol#L104)과 같은 각 관련 자산 컨트랙트에 새로운 방법의 도입이 포함됩니다. 그리고 `Marketplace.sol` 스마트 컨트랙트는 스왑을 실행합니다.


## transferWithSig 메소드

메소드 정의는 다음과 같습니다:
```javascript
function transferWithSig(bytes calldata sig, uint256 amount, bytes32 data, uint256 expiration, address to) external returns (address from) {
    require(amount > 0);
    require(expiration == 0 || block.number <= expiration, "Signature is expired");

    bytes32 dataHash = getTokenTransferOrderHash(
      msg.sender,
      amount,
      data,
      expiration
    );
    require(disabledHashes[dataHash] == false, "Sig deactivated");
    disabledHashes[dataHash] = true;

    from = ecrecovery(dataHash, sig);
    _transferFrom(from, to, amount);
}
```

### 매개변수들
`bytes calldata sig` - 다른 토큰 세트와 교환하여 정해진 양의 토큰을 사용하는 주문에 대한 사용자의 서명(*주문* 생성)

`uint256 amount` - 사용자가 서명한 토큰의 수량

`bytes32 data`는 일치하는 순서(주문 ID, 토큰, 수량)의 `keccak256` 해시

`uint256 expiration` - 주문이 만료될 블록 번호

`address to` - 주문 필러의 주소

위의 방법은 외부 컨트랙트에서 호출될 때 전달된 서명의 유효성을 검사합니다. 구성은 [EIP712](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md)를 따르고 사용자 주소를 복구하고 지정된 수량의 토큰을 사용자 계정에서 지정된 계정으로 전송합니다.

## 자산 스왑 프로토콜

이제 서명을 사용하여 사용자 계정에서 자산을 전송하는 이 특정 기능을 여러 가지 방법으로 사용할 수 있습니다. 그 중 하나는 토큰 간의 원자적 스왑을 실행하는 DEX와 같은 마켓플레이스입니다.

### Terminology(술어)
**주문**은 주문 ID, 토큰 주소, 수량(또는 토큰 ID)으로 구성됩니다. 사용자가 **주문**에 서명하고 서명을 생성합니다. 그런 다음 이 서명은 사용자를 대신하여 서명된 수량의 자산을 전송하는 데 사용됩니다.

다음은 자산의 원자적 스왑을 수행하는 폴리곤 체인에 배포된 [마켓 플레이스](https://github.com/maticnetwork/contracts/blob/master/contracts/child/misc/Marketplace.sol) 스마트 컨트랙트의 세부 사양입니다.

### Marketplace.sol

```javascript
pragma solidity ^0.5.2;

interface MarketplaceToken {
  function transferWithSig(bytes calldata sig, uint256 tokenIdOrAmount, bytes32 data, uint256 expiration, address to) external returns (address);
}

contract Marketplace {
  struct Order {
    address token;
    bytes sig;
    uint256 tokenIdOrAmount;
  }

  function decode(bytes memory data) internal pure returns(Order memory order) {
    (order.token, order.sig, order.tokenIdOrAmount) = abi.decode(data, (address, bytes, uint256));
  }

  function executeOrder(
    bytes memory data1,
    bytes memory data2,
    bytes32 orderId,
    uint256 expiration,
    address taker
  ) public {
    Order memory order1 = decode(data1);
    Order memory order2 = decode(data2);

    // Transferring order1.token tokens from tradeParticipant1 to address2
    address tradeParticipant1 = MarketplaceToken(order1.token).transferWithSig(
      order1.sig,
      order1.tokenIdOrAmount,
      keccak256(abi.encodePacked(orderId, order2.token, order2.tokenIdOrAmount)),
      expiration,
      taker
    );

    // Transferring token2 from tradeParticipant2 to tradeParticipant1
    address tradeParticipant2 = MarketplaceToken(order2.token).transferWithSig(
      order2.sig,
      order2.tokenIdOrAmount,
      keccak256(abi.encodePacked(orderId, order1.token, order1.tokenIdOrAmount)),
      expiration,
      tradeParticipant1
    );
    require(taker == tradeParticipant2, "Orders are not complimentary");
  }
}
```
위의 컨트랙트는 사전 승인 또는 허용 트랜잭션 없이 스왑을 수행합니다.

executeOrder 함수는 주문 필러(주문을 이행하는 참가자)와 함께 결제될 두 개의 주문을 나타내는 두 개의 바이트스트림을 사용합니다. 주문 결제는 두 주문의 두 토큰에 대해 transferWithSig 메소드를 실행하여 발생합니다:

```javascript
address tradeParticipant1 = MarketplaceToken(order1.token).transferWithSig(
      order1.sig,
      order1.tokenIdOrAmount,
      keccak256(abi.encodePacked(orderId, order2.token, order2.tokenIdOrAmount)),
      expiration,
      taker
    );

// Transferring token2 from tradeParticipant2 to tradeParticipant1
address tradeParticipant2 = MarketplaceToken(order2.token).transferWithSig(
      order2.sig,
      order2.tokenIdOrAmount,
      keccak256(abi.encodePacked(orderId, order1.token, order1.tokenIdOrAmount)),
      expiration,
      tradeParticipant1
    );
require(taker == tradeParticipant2, "Orders are not complimentary");
```

## 튜토리얼 (ERC20/721 스왑)

다음은 폴리곤에서 플라즈마 지원 자산 스왑 실행을 시도할 수 있는 짧은 튜토리얼입니다. [여기](https://github.com/nglglhtr/asset-swap-tutorial)에서 복제할 상용구 코드베이스가 준비되었습니다. 리포지토리는 모든 관련 컨트랙트(ChildERC20, ChildERC721, Marketplace 및 해당 종속물)과 앞으로 튜토리얼을 안내할 스크립트로 구성됩니다.

### 전제조건
1. 노드 v10.17.0(npm v6.11.3)을 사용하는 것이 가장 좋습니다
2. Truffle
```
npm install -g truffle
```
3. Web3
```
npm install -g web3
```

리포지토리 복제 및 종속물 설치

```bash
$ git clone https://github.com/nglglhtr/asset-swap-tutorial.git
$ cd asset-swap-tutorial
$ npm i
```

> 참고: 폴리곤에 매핑된 모든 토큰(매핑은 메인 체인 또는 루트 체인 간에 자산 이동을 가능하게 함)은 [ChildERC20](https://github.com/maticnetwork/contracts/blob/master/contracts/child/ChildERC20.sol) 및 [ChildERC721](https://github.com/maticnetwork/contracts/blob/master/contracts/child/ChildERC721.sol) 토큰의 형태로 폴리곤 사이드체인에 배포됩니다.

이 튜토리얼에서 사용된 ChildERC20 및 ChildERC721 버전에는 하나의 추가 기능이 포함되어 있습니다:

```javascript
// ChildERC20
function mint (uint256 amount) public {
    _mint (msg.sender, amount);
}
```
```javascript 
// ChildERC721
function mint (uint256 tokenId) public {
    _mint (msg.sender, tokenId);
}
```
이는 스왑을 수행하기 전에 필요한 토큰을 발행하는 데 도움이 됩니다.

### Step 1 - 설정
#### 1 – 컨트랙트 컴파일 및 배포하기
저장소를 복제했으면 컨트랙트를 컴파일하고 원하는 네트워크로 마이그레이션하십시오.

```bash
$ truffle compile
$ truffle migrate
```
디렉토리를 스크립트로 변경합니다.

`scripts/erc20-721/` 디렉토리로 `cd`.

#### 2 – 컨트랙트 및 계정의 세부 정보 입력하기
`/scripts/erc20-721/` 디렉토리 아래에 있는 `config.js` 파일을 열고 언급된 변수 값을 입력합니다.

`provider` - 컨트랙트가 배포된 네트워크 공급자

`erc20` - erc20 컨트랙트의 주소

`erc721` - erc721 컨트랙트의 주소

`마켓플레이스` - 마켓플레이스 컨트랙트의 주소

`amount` - `tokenid`로 교환하려는 erc20 토큰의 수량

`tokenid` - erc20 토큰 `수량`과 교환하려는 erc721 토큰의 ID

`privateKey1` 및 `privateKey2` - 스왑에 참여하는 계정의 프라이빗 키

지금은 `orderId` 및 `expiration`을 그대로 둘 수 있습니다.

> 참고: 프로덕션용으로 빌드할 때 코드에 프라이빗 키를 하드코딩하는 대신 지갑을 사용하는 것이 가장 좋습니다.

### Step 2 – 발행

#### 1 – 두 계정으로 토큰 발행하기
실행
```bash
$ node mint.js
```
두 계정에서 토큰을 발행하기 위해.


스크립트의 다음 함수는 첫 번째 계정에서 지정된 수량의 토큰을 발행하고 두 번째 계정에서 지정된 tokenId의 NFT를 발행합니다.

```javascript
async function mint () {
    await CHE.methods.mint(config.amount).send({
        from: wallet[0].address,
        gas: 6721975
    }).on('transactionHash', function(transactionHash){ console.log("erc20 mint\t" +  transactionHash) })

    await NFT.methods.mint(config.tokenid).send({
        from: wallet[1].address,
        gas: 6721975
    }).on('transactionHash', function(transactionHash){ console.log("erc721 mint\t" +  transactionHash) })
}
```

스크립트를 실행하면 두 발행의 트랜잭션 해시가 표시됩니다.

다음을 실행하여 언제든지 두 계정의 잔고를 볼 수 있습니다:
```bash
$ node balance.js
```
그러면 두 토큰에 대한 두 계정의 잔액이 표시됩니다.

### Step 3 - 스왑

두 계정을 교환하기 위해 먼저 두 개의 서명을 생성합니다. 이는 두 개의 주문을 생성하는 것과 같습니다.


스크립트에서 `swap.js` `encode` 함수는 `Marketplace.sol` 스마트 컨트랙트에서 `executeOrder` 함수의 처음 두 매개변수인 데이터의 바이트스트림을 준비합니다.
```javascript
function encode(token, sig, tokenIdOrAmount) {
    return web3.eth.abi.encodeParameters(
      ['address', 'bytes', 'uint256'],
      [token, sig, '0x' + tokenIdOrAmount.toString(16)]
    )
}
```

다음으로 두 개의 서명 개체를 만듭니다. 기본적으로 마켓플레이스 스마트 컨트랙트를 통해 일치시키고 실행하려는 두 개의 주문입니다.

```javascript
const obj1 = sigUtils.getSig({
    privateKey: privateKey1,
    spender: marketplaceAddress,
    orderId: orderId,
    expiration: expiration,

    token1: token1,
    amount1: amount1,
    token2: token2,
    amount2: amount2
})

const obj2 = sigUtils.getSig({
    privateKey: privateKey2,
    spender: marketplaceAddress,
    orderId: orderId,
    expiration: expiration,

    token2: token1,
    amount2: amount1,
    token1: token2,
    amount1: amount2
})
```

그리고 두 가지 명령을 실행합니다:

```javascript
Marketplace.methods.executeOrder(
    encode(token1, obj1.sig, amount1),
    encode(token2, obj2.sig, amount2),
    orderId,
    expiration,
    address2
).send({
    from: address3,
    gas: maxGas
}).then(console.log)
```

스왑을 실행하기 위해 다음을 실행합니다:
```bash
$ node swap.js
```
성공적인 스왑은 트랜잭션 해시를 표시합니다. 다음으로 잔고를 확인할 수 있습니다 -

```bash
$ node balance.js
```

### 폴리곤에서 배포 및 스왑하기

폴리곤에서 배포 및 테스트하려는 경우 단계는 스마트 컨트랙트를 폴리곤으로 마이그레이션하고 구성 파일에서 컨트랙트 주소를 변경하는 것뿐입니다.

루트 디렉터리에서 다음을 실행합니다:
```bash
$ truffle migrate --network maticTestnet
```
또는 Matic 베타 네트워크에서 다음을 실행합니다:
```bash
$ truffle migrate --network maticBetaMainnet
```

컨트랙트 주소가 있으면 `/scripts/erc20-721/` 아래의 구성 파일을 두 네트워크에 대해 다음과 같은 공급자와 함께 채우십시오:

폴리곤 테스트넷: `<Mumbai testnet RPC URL> https://rpc.maticvigil.com/ 또는 기타 호스팅 노드 공급자에서 무료 전용 RPC URL에 가입하세요.`

폴리곤 베타 메인넷: `https://beta.matic.network`

구성파일이 준비되면, inside the `/scripts/erc20-721/` 에서 다음을 실행합니다.-

토큰을 발행하기 위해
```bash
$ node mint.js
```
잔고를 확인하기 위해
```bash
$ node balance.js
```
스왑하기 위해
```bash
$ node swap.js
```
스왑이 성공하면 잔고를 다시 체크하고 확인할 수 있습니다.
```bash
$ node balance.js
```
