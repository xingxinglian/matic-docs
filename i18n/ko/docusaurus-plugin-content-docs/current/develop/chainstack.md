---
id: chainstack
title: Chainstack 이용하기
sidebar_label: Chainstack 이용하기
description: 폴리곤에서 다음 블록체인 앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

# Hello World Smart Contract on Polygon

이 섹션에서는 뭄바이 테스트넷에서 [Chainstack](https://chainstack.com/build-better-with-polygon/) 및 [Foundry](https://github.com/gakonst/foundry/)를 사용하여 Hello World 컨트랙트를 배포하는 방법을 안내합니다.

질문이 있는 경우 [Chainstack Discord](https://discord.com/invite/Cymtg2f7pX)에 문의하세요.

## 폴리곤 뭄바이 노드 배포

스마트 컨트랙트를 블록체인 네트워크에 배포하려면 노드가 필요합니다. 노드를 가져오는 단계를 따르세요:

1. [Chainstack에 가입하기](https://console.chainstack.com/user/account/create).
1. [뭄바이 노드 배포하기](https://docs.chainstack.com/platform/join-a-public-network#join-a-polygon-pos-network).
1. [배포된 노드의 HTTPS 엔드포인트 가져오기](https://docs.chainstack.com/platform/view-node-access-and-credentials).

## Foundry 설치

Foundry는 스마트 컨트랙트와 함께 작동하는 개발 툴킷입니다.

1. [Rust 설치](https://www.rust-lang.org/tools/install).
1. [Foundry 설치](https://github.com/gakonst/foundry/).

## Foundry로 초기화

상용구 프로젝트를 만들려면 작업 디렉터리로 이동하여 다음을 실행합니다:

``` sh
forge init PROJECT_NAME
```

다음에서

* PROJECT_NAME - 프로젝트 이름

## 계정에 입금

컨트랙트를 배포하려면 네트워크에서 가스를 지불해야 합니다.

[faucet를 통해](https://faucet.polygon.technology/) 뭄바이 MATIC 얻기.

## Hello World 컨트랙트 생성하기

`src/`의 초기화된 Foundry 프로젝트에서, `HelloWorld.sol`을 생성합니다:

```
// SPDX-License-Identifier: None

// Specifies the version of Solidity, using semantic versioning.
// Learn more: https://solidity.readthedocs.io/en/v0.5.10/layout-of-source-files.html#pragma
pragma solidity >=0.8.9;

// Defines a contract named `HelloWorld`.
// A contract is a collection of functions and data (its state). Once deployed, a contract resides at a specific address on the Ethereum blockchain. Learn more: https://solidity.readthedocs.io/en/v0.5.10/structure-of-a-contract.html
contract HelloWorld {

   //Emitted when update function is called
   //Smart contract events are a way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen.
   event UpdatedMessages(string oldStr, string newStr);

   // Declares a state variable `message` of type `string`.
   // State variables are variables whose values are permanently stored in contract storage. The keyword `public` makes variables accessible from outside a contract and creates a function that other contracts or clients can call to access the value.
   string public message;

   // Similar to many class-based object-oriented languages, a constructor is a special function that is only executed upon contract creation.
   // Constructors are used to initialize the contract's data. Learn more:https://solidity.readthedocs.io/en/v0.5.10/contracts.html#constructors
   constructor(string memory initMessage) {

      // Accepts a string argument `initMessage` and sets the value into the contract's `message` storage variable).
      message = initMessage;
   }

   // A public function that accepts a string argument and updates the `message` storage variable.
   function update(string memory newMessage) public {
      string memory oldMsg = message;
      message = newMessage;
      emit UpdatedMessages(oldMsg, newMessage);
   }
}
```

## 컨트랙트 배포하기

이 시점에서 스마트 컨트랙트를 배포할 준비가 되었습니다:

* 컨트랙트를 배포할 폴리곤 뭄바이 네트워크에 자체 노드가 있습니다.
* 컨트랙트를 배포하는데 사용할 Foundry가 있습니다.
* 컨트랙트를 배포할 입금된 계정이 있습니다.

컨트랙트를 배포하려면 다음을 실행하세요:

``` sh
forge create HelloWorld --constructor-args "Hello" --contracts CONTRACT_PATH --private-key PRIVATE_KEY --rpc-url HTTPS_ENDPOINT
```

다음에서

* CONTRACT_PATH — `HelloWorld.sol` 파일의 경로.
* PRIVATE_KEY — 계정의 프라이빗 키.
* HTTPS_ENDPOINT — [노드의 엔드포인트](https://docs.chainstack.com/platform/view-node-access-and-credentials)입니다.

예시:

``` sh
forge create HelloWorld --constructor-args "Hello" --contracts /root/foundry/src/HelloWorld.sol --private-key d8936f6eae35c73a14ea7c1aabb8d068e16889a7f516c8abc482ba4e1489f4cd --rpc-url https://nd-123-456-789.p2pify.com/3c6e0b8a9c15224a8228b9a98ca1531d
```

축하합니다! Hello World 스마트 컨트랙트를 폴리곤에 배포했습니다!

더 많은 [튜토리얼](https://docs.chainstack.com/tutorials/polygon/)과 [도구들](https://docs.chainstack.com/operations/polygon/tools)는 Chainstack 문서를 참조하십시오.
