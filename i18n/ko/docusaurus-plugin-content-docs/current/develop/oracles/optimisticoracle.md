---
id: optimisticoracle
title: UMA의 낙관적 오라클
sidebar_label: UMA
description: 효율적인 Oracle 솔루션을 위해 낙관적인 합의를 활용합니다.
keywords:
  - docs
  - oracle
  - UMA
image: https://matic.network/banners/matic-network-16x9.png
---

## 시작하기
UMA의 오라클 시스템은 두 가지 핵심 구성 요소로 구성됩니다:

1. Optimistic Oracle

2. 데이터 검증 메커니즘(DVM)

## Optimistic Oracle

MA의 Optimistic Oracle을 사용하면 컨트랙트에서 가격 정보를 신속하게 요청하고 받을 수 있습니다. Optimistic Oracle은 가격 요청을 시작하는 컨트랙트와 DVM(Data Verification Mechanism)으로 알려진 UMA의 분쟁 해결 시스템 간의 일반화된 에스컬레이션 게임 역할을 합니다. Optimistic Oracle이 제안한 가격은 분쟁이 없는 한 DVM으로 전송되지 않습니다. 이를 통해 컨트랙트는 자산 가격을 온체인에 기록할 필요 없이 사전 정의된 시간 내에 가격 정보를 얻을 수 있습니다.

## 데이터 검증 메커니즘(DVM)

분쟁이 발생하면 요청이 DVM으로 전송됩니다. UMA를 기반으로 구축된 모든 컨트랙트는 DVM을 분쟁 해결을 위한 백스톱으로 사용합니다. DVM으로 전송된 분쟁은 UMA 토큰 소유자가 주어진 시간에 자산 가격에 투표한 후 48시간 후에 해결됩니다. UMA에 대한 컨트랙트는 48시간보다 빠른 자산 가격을 요구하지 않는 한 Optimistic Oracle을 사용할 필요가 없습니다.

DVM(Data Verification Mechanism)은 UMA 프로토콜을 기반으로 구축된 컨트랙트에 대한 분쟁 해결 서비스입니다. DVM은 변동성(때로는 조작 가능한) 시장에서 문제가 발생할 때 컨트랙트가 안전하고 올바르게 관리되도록 보장하기 위해 인간 판단의 요소를 포함하기 때문에 강력합니다.

## Optimistic Oracle 인터페이스

대다수의 프로젝트는 구현을 위해 Optimistic Oracle만 필요합니다.

이 섹션에서는 다양한 참가자가 Optimistic Oracle과 상호 작용할 수 있는 방법을 설명합니다. Optimistic Oracle 컨트랙트의 최신 메인넷, kovan 또는 L2 배포를 보려면 [프로덕션 주소](https://docs.umaproject.org/dev-ref/addresses)를 참조하십시오.

Optimistic Oracle은 파이낸셜 컨트랙트 또는 제3자가 가격을 검색하는 데 사용합니다. 가격이 요청되면 누구나 그에 대한 응답으로 가격을 제안할 수 있습니다. 일단 제안되면, 가격은 제안된 가격에 대해 누구나 이의를 제기할 수 있는 활성 기간을 거칩니다.

Optimistic Oracle 인터페이스를 구성하는 12가지 방법이 있습니다.
-  `requestPrice`
-  `proposePrice`
-  `disputePrice`
-  `settle`
-  `hasPrice`
-  `getRequest`
-  `settleAndGetPrice`
-  `setBond`
-  `setCustomLiveness`
-  `setRefundOnDispute`
-  `proposePriceFor`
-  `disputePriceFor`

### requestPrice

새로운 가격을 요청합니다. 등록된 가격 식별자에 대한 것이어야 합니다. 이것은 UMA 시스템에 등록된 대부분의 금융 계약에서 자동으로 호출되지만 등록된 가격 식별자에 대해 누구나 호출할 수 있습니다. 예를 들어, Expiring Multiparty(EMP) 컨트랙트는 `expire` 메소드가 호출될 때 이 메소드를 호출합니다.

매개변수:
- `identifier`: 요청되는 가격 식별자.
- `timestamp`: 요청되는 가격의 타임스탬프.
- `ancillaryData`: 가격 요청과 함께 전달되는 추가 인수를 나타내는 보조 데이터.
- `currency`: 보상 및 수수료 지불에 사용되는 ERC20 토큰. DVM과 함께 사용하려면 승인을 받아야 합니다.
- `reward`: 성공적인 제안자에게 제공되는 보상. 발신자가 지불합니다. 참고: 이것은 0일 수 있습니다.

### proposePrice

기존 가격 요청에 대한 가격 값을 제안합니다.

매개변수:
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.
- `proposedPrice`: 제안된 가격.

### disputePrice

활성 제안서가 있는 기존 가격 요청에 대한 가격 값에 대해 이의를 제기합니다.

매개변수:
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### settle

미결제 가격 요청을 결제하려고 시도합니다. 결제되지 않으면 되돌립니다.

매개변수:
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### hasPrice

주어진 요청이 해결되었거나 해결되었는지 확인합니다(예: optimistic oracle에는 가격이 있음).

매개변수:
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### getRequest

가격 요청에 대한 모든 정보를 포함하는 현재 데이터 구조를 가져옵니다.

매개변수:
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### settleAndGetPrice

이전에 호출자가 요청한 가격을 검색합니다. 요청이 해결되지 않거나 해결할 수 없는 경우 되돌립니다. 참고: 이 메소드는 보여주기 위한 것이 아니므로 이 호출이 가격 요청이 아직 정산되지 않은 경우 실제로 정산할 수 있습니다.

매개변수:
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### setBond

가격 요청과 관련된 제안 보증금을 설정합니다.

매개변수:
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.
- `bond`: 설정할 맞춤형 보증금액.

### setCustomLiveness

요청에 대한 사용자 정의 liveness 값을 설정합니다. liveness는 제안이 자동 해결되기 전에 기다려야 하는 시간입니다.

매개변수:
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.
- `customLiveness`: 새로운 사용자 정의 liveness.

### setRefundOnDispute

제안에 이의가 있는 경우 보상을 환불하도록 요청을 설정합니다. 이것은 분쟁으로 인한 지연의 경우 호출자를 "헤지"하는 데 도움이 될 수 있습니다. 참고: 분쟁이 발생할 경우 승자는 여전히 상대방의 자금을 받으므로 보상을 환불 받더라도 여전히 수익이 발생합니다.

매개변수:
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### disputePriceFor

다른 주소를 대신하여 활성 제안으로 가격 요청에 이의를 제기합니다. 참고: 이 주소는 이 분쟁으로 인해 발생하는 모든 보상을 받게 됩니다. 그러나 모든 자금은 호출자로부터 당겨집니다.

매개변수:
- `disputer`: 분쟁자로 설정할 주소.
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.

### proposePriceFor

다른 주소를 대신하여 가격 값을 제안합니다. 참고: 이 주소는 이 제안에서 오는 모든 보상을 받습니다. 그러나 모든 채권은 호출자로부터 당겨집니다.

매개변수:
- `proposer`: 제안자로 설정할 주소.
- `requester`: 초기 가격 요청의 발신자.
- `identifier`: 기존 요청을 식별하기 위한 가격 식별자.
- `timestamp`: 기존 요청을 식별하기 위한 타임스탬프.
- `ancillaryData`: 요청되는 가격의 보조 데이터.
- `proposedPrice`: 제안된 가격.

# Optimistic Oracle 통합하기

이 데모는 사용자의 ERC-20 토큰 잔고를 관리하는 `OptimisticDepositBox` 컨트랙트를 설정합니다.

로컬 테스트넷 블록체인에서 사용자는 wETH(Wrapped Ether)를 계약에 입금하고 USD로 표시된 wETH를 인출합니다. 예를 들어 사용자가 $10,000 USD의 wETH를 인출하기를 원하고 ETH/USD 환율이 $2,000인 경우 5wETH를 인출하게 됩니다.

* 사용자는 `OptimisticDepositBox`를 DVM에서 활성화된 가격 식별자 중 하나와 연결합니다.

* 사용자는 `OptimisticDepositBox`에 wETH를 입금하고 `ETH/USD` 가격 식별자로 등록합니다.

* 사용자는 이제 스마트 컨트랙트 호출을 통해 `DepositBox`에서 USD로 표시된 금액의 wETH를 인출할 수 있으며 Optimistic Oracle은 낙관적인 온체인 가격 책정을 가능하게 합니다.

이 예에서 사용자는 오프체인 `ETH/USD` 가격 피드를 참조하지 않고는 USD 표시 금액의 wETH를 전송할 수 없었을 것입니다. 따라서 Optimistic Oracle은 사용자가 참조 가격을 "끌어올" 수 있도록 합니다.

DVM에 대한 가격 요청과 달리 Optimistic Oracle에 대한 가격 요청은 분쟁이 없는 경우 지정된 활성 기간 내에서 해결할 수 있으며 DVM 투표 기간보다 훨씬 짧을 수 있습니다. 활성 기간은 구성할 수 있지만 DVM을 통한 결제의 경우 2-3일에 비해 일반적으로 2시간입니다.

가격 요청자는 현재 DVM에 수수료를 지불할 필요가 없습니다. 요청자는 가격 요청에 응답하는 제안자에 대해 보상을 제공할 수 있지만 이 예에서는 보상 값이 `0`으로 설정됩니다.

가격 제안자는 가격과 함께 보증금을 게시하며 가격에 이의가 없거나 제안자에게 유리하게 분쟁이 해결된 경우 환불됩니다. 그렇지 않으면, 이 보증금은 DVM에 최종 수수료를 지불하고 성공적인 분쟁자에게 보상을 지불하는 데 사용됩니다.

데모에서 요청자는 가격 제안자로부터 추가 보증금을 요구하지 않으므로 게시된 총 보증금은 현재 0.2 wETH의 wETH 최종 수수료와 같습니다. 구현 세부 사항은 `OptimisticOracle` [컨트랙트](https://docs-dot-uma-protocol.appspot.com/uma/contracts/OptimisticOracle.html)의 `proposalPriceFor` 함수를 참조하십시오.

## 데모 실행하기

1. [여기](https://docs.umaproject.org/developers/setup)에서 모든 전제 조건 설정 단계를 따랐는지 확인하십시오.
2. `yarn ganache-cli --port 9545`를 사용하여 로컬 Ganache 인스턴스(예: Kovan/Ropsten/Rinkeby/Mainnet 아님)를 실행합니다.
3. 다른 창에서 다음 명령을 실행하여 컨트랙트를 마이그레이션합니다:

```bash
yarn truffle migrate --reset --network test
```

1. `OptimisticDepositBox` [컨트랙트](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/financial-templates/demo/OptimisticDepositBox.sol)를 배포하고 간단한 사용자 흐름을 진행하려면 리포지토리의 루트에서 다음 데모 스크립트를 실행합니다:

```bash
yarn truffle exec ./packages/core/scripts/demo/OptimisticDepositBox.js --network test
```

다음 출력을 볼 수 있습니다:

```
1. Deploying new OptimisticDepositBox
  - Using wETH as collateral token
  - Pricefeed identifier for ETH/USD is whitelisted
  - Collateral address for wETH is whitelisted
  - Deployed an OptimisticOracle
  - Deployed a new OptimisticDepositBox Minting ERC20 to user and giving OptimisticDepositBox allowance to transfer collateral
  - Converted 10 ETH into wETH
  - User's wETH balance: 10
  - Increased OptimisticDepositBox allowance to spend wETH
  - Contract's wETH allowance: 10


3. Depositing ERC20 into the OptimisticDepositBox
  - Deposited 10 wETH into the OptimisticDepositBox
  - User's deposit balance: 10
  - Total deposit balance: 10
  - User's wETH balance: 0


4. Withdrawing ERC20 from OptimisticDepositBox
  - Submitted a withdrawal request for 10000 USD of wETH
  - Proposed a price of 2000000000000000000000 ETH/USD
  - Fast-forwarded the Optimistic Oracle and Optimistic Deposit Box to after the liveness window so we can settle.
  - New OO time is [fast-forwarded timestamp]
  - New ODB time is [fast-forwarded timestamp]
  - Executed withdrawal. This also settles and gets the resolved price within the withdrawal function.
  - User's deposit balance: 5
  - Total deposit balance: 5
  - User's wETH balance: 5
```

## 컨트랙트 함수 설명

`OptimisticDepositBox` [컨트랙트 코드](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/financial-templates/demo/OptimisticDepositBox.sol)는 Oracle과 상호 작용하는 방법입니다.

`constructor` 함수에는 `OptimisticOracle` 주소, 승인된 담보 및 가격 식별자 화이트리스트, 기타 중요한 계약 주소의 레지스트리를 유지 관리하는 UMA `Finder` 컨트랙트에 대한 `_finderAddress` 인수가 포함됩니다.

이를 통해 `constructor` 는 담보 유형 및 가격 식별자가 유효한지 확인하고 `OptimisticDepositBox`가 나중에 `OptimisticOracle`을 찾아 상호 작용할 수 있습니다.

`requestWithdrawal` 함수에는 `ETH/USD` 가격을 요청하는 `OptimisticOracle`에 대한 내부 호출이 포함됩니다. 반환되면 사용자는 `executeWithdrawal`을 호출하여 출금을 완료할 수 있습니다.

코드 주석에 훨씬 더 많은 정보와 설명이 있으니 더 자세히 알고 싶다면 살펴보세요!

# 추가 리소스

다음은 UMA DVM과 관련된 몇 가지 추가 리소스입니다:

- [Technical Architecture](https://docs.umaproject.org/oracle/tech-architecture)
- [Economic Architecture](https://docs.umaproject.org/oracle/econ-architecture)
- UMA’s DVM설계에 대한 [블로그 게시물](https://medium.com/uma-project/umas-data-verification-mechanism-3c5342759eb8)
- UMA’s DVM설계에 대한 [백서](https://github.com/UMAprotocol/whitepaper/blob/master/UMA-DVM-oracle-whitepaper.pdf)
- 최적의 수수료 정책을 위한 [Research repo](https://github.com/UMAprotocol/research)
- 거버넌스 제안을 위한 [UMIP repo](https://github.com/UMAprotocol/UMIPs)
