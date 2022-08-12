---
title: Tellor
description: Tellor 오라클을 폴리곤 컨트랙트에 통합하기를 시작하기 위한 가이드
author: "Tellor"
lang: en
sidebar: true
tags:
  - "solidity"
  - "smart contracts"
  - "price feeds"
  - "oracle"
  - "폴리곤"
  - "Matic"
  - "Tellor"
skill: beginner
published: 2022-02-10
source: Tellor Docs
sourceUrl: https://docs.tellor.io/tellor/
---

Tellor는 단순한 암호화-경제 인센티브로 보호되는 검열에 강한 데이터를 제공하는 오라클입니다. 데이터는 누구나 제공할 수 있고 누구나 확인할 수 있습니다. Tellor의 유연한 구조는 모든 데이터를 시간 간격으로 제공할 수 있어 실험/혁신이 용이합니다.

## (소프트)전제조건

오라클 측면에 집중하기 위해 당신의 코딩 기술 수준에 대해 다음을 가정합니다.

가정:

- 터미널을 탐색할 수 있습니다.
- npm이 설치되어 있습니다.
- npm을 사용하여 종속물들을 관리하는 방법을 알고 있습니다.

Tellor는 구현 준비가 된 라이브 오픈 소스 오라클입니다. 이 초보자 가이드는 사람들이 Tellor를 시작하고 실행할 수 있는 용이함을 보여주고, 프로젝트에 완전히 분산되고 검열에 강한 오라클을 제공합니다.

## 개요

Tellor는 당사자가 오프체인 데이터 포인트(예: BTC/USD)의 가치를 요청할 수 있고 기자가 이 값을 모든 폴리곤 스마트 컨트랙트에서 액세스할 수 있는 온체인 데이터 뱅크에 추가하기 위해 경쟁하는 오라클 시스템입니다. 이 데이터 뱅크에 대한 입력은 스테이크된 리포터 네트워크에 의해 보호됩니다. Tellor는 암호화-경제 인센티브 메커니즘을 활용합니다. 리포터의 정직한 데이터 제출은 Tellor의 토큰 발행으로 보상됩니다. 모든 나쁜 행위자는 분쟁 메커니즘에 의해 신속하게 처벌되고 네트워크에서 제거됩니다.

이 자습서에서는 다음을 살펴보겠습니다.

- 시작하고 실행하는 데 필요한 초기 툴킷 설정.
- 간단한 예를 살펴보십시오.
- 현재 Tellor를 테스트할 수 있는 네트워크의 테스트넷 주소를 나열합니다.

## UsingTellor

가장 먼저 해야 할 일은 Tellor를 오라클로 사용하는 데 필요한 기본 도구를 설치하는 것입니다. [이 패키지](https://github.com/tellor-io/usingtellor)를 사용하여 Tellor 사용자 컨트랙트를 설치하십시오:

`npm install usingtellor`

일단 설치되면 당신의 컨트랙트가 'UsingTellor' 컨트랙트에서 함수들을 넘겨 받을 수 있습니다.

대단합니다! 이제 도구가 준비되었으므로 비트코인 가격을 검색하는 간단한 연습을 진행해 보겠습니다:

### BTC/USD 예시

UsingTellor 컨트랙트를 넘겨 받고 Tellor 주소를 constructor 인수로 전달합니다.

다음은 예입니다:

```solidity
import "usingtellor/contracts/UsingTellor.sol";

contract BtcPriceContract is UsingTellor {

  //This Contract now has access to all functions in UsingTellor

  bytes btcPrice;
  bytes32 btcQueryId = 0x0000000000000000000000000000000000000000000000000000000000000002;

  constructor(address payable _tellorAddress) UsingTellor(_tellorAddress) public {}

  function setBtcPrice() public {
    bool _didGet;
    uint256 _timestamp;

    (_didGet, btcPrice, _timestamp) = getCurrentValue(btcQueryId);
  }
}
```

**다른 데이터 피드를 시도하고 싶으십니까? 여기에서 지원되는 데이터 피드 목록을 확인하세요: [Current Data Feeds](https://docs.tellor.io/tellor/integration/data-feed-ids)**

## 주소들:

Tellor Tributes: [`0xe3322702bedaaed36cddab233360b939775ae5f1`](https://polygonscan.com/token/0xe3322702bedaaed36cddab233360b939775ae5f1#code)

Oracle: [`0xfd45ae72e81adaaf01cc61c8bce016b7060dd537`](https://polygonscan.com/address/0xfd45ae72e81adaaf01cc61c8bce016b7060dd537#code)

#### 먼저 몇 가지 테스트를 하시겠습니까?:

Polygon Mumbai Testnet: [`0x3477EB82263dabb59AC0CAcE47a61292f28A2eA7`](https://mumbai.polygonscan.com/address/0x3477EB82263dabb59AC0CAcE47a61292f28A2eA7/contracts#code)

#### Tellor 오라클의 보다 강력한 구현을 위해 사용 가능한 전체 함수 목록을 [여기](https://github.com/tellor-io/usingtellor/blob/master/README.md)에서 확인하십시오.
