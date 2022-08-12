---
id: erc20
title: ERC20 입출금 가이드
sidebar_label: ERC20
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## 높은 수준의 작업흐름

ERC20 입금하기

1. 예치해야 하는 토큰을 사용하기 위해 **_ERC20Predicate_** 컨트랙트를 **_승인합니다_**.
2. **_RootChainManager_**에 **_depositFor_**를 호출합니다.

ERC20 출금하기 -

1. 폴리곤 체인에서 토큰을 **_소각합니다_**.
2. **_RootChainManager_**에서 **_exit_** 함수를 호출하여 소각 트랜잭션의 증명을 제출하십시오. 이 호출은 소각 트랜잭션이 포함된 블록에 대한 **_체크포인트가 제출된 후_**에 만들 수 있습니다.

## 단계 세부정보
---

### 컨트랙트 인스턴스화하기
```js
const mainWeb3 = new Web3(mainProvider)
const maticWeb3 = new Web3(maticProvider)
const rootTokenContract = new mainWeb3.eth.Contract(rootTokenABI, rootTokenAddress)
const rootChainManagerContract = new mainWeb3.eth.Contract(rootChainManagerABI, rootChainManagerAddress)
const childTokenContract = new maticWeb3(childTokenABI, childTokenAddress)
```

### 승인하기
**_ERC20Predicate_**가 토큰 컨트랙트의 **_approve_**함수를 호출함으로써 토큰을 사용하도록 승인합니다. 이 함수는 두 개의 인수와 금액을 사용합니다. **_spender_**는 사용자의 토큰 사용 승인을 받고 있는 주소입니다. **_amount_**는 사용할 수 있는 토큰의 수량입니다. 1회 승인 시 입금액과 동일한 금액을 유지하거나 여러 번 승인하지 않도록 더 큰 숫자를 전달하십시오.
```js
await rootTokenContract.methods
  .approve(erc20Predicate, amount)
  .send({ from: userAddress })
```

### 입금하기
이 호출을 하기 전에 토큰을 매핑해야 하고 금액을 입금 승인을 받아야 합니다.  
**_RootChainManager_** 컨트랙트의 **_depositEtherFor_** 함수를 호출합니다. 이 함수는 3개의 인수 user, rootToken 및 depositData를 갖고 있습니다. **_user_**는 폴리곤 체인에 입금을 받을 사용자의 주소입니다.  **_rootToken_** 은 메인 체인의 토큰 주소입니다. **_depositData_** 는 abi로 인코딩된 수량입니다.
```js
const depositData = mainWeb3.eth.abi.encodeParameter('uint256', amount)
await rootChainManagerContract.methods
  .depositFor(userAddress, rootToken, depositData)
  .send({ from: userAddress })
```

### 소각하기
토큰은 하위 토큰 계약에서 **_withdraw_** 함수를 호출하여 폴리곤 체인에서 소각될 수 있습니다. 이 함수는 소각할 토큰 수를 나타내는 **_amount_** 단일 인수를 사용합니다. 이 소각의 증명은 종료 단계에서 제출되어야 합니다. 따라서 트랜잭션 해시를 저장합니다.
```js
const burnTx = await childTokenContract.methods
  .withdraw(amount)
  .send({ from: userAddress })
const burnTxHash = burnTx.transactionHash
```

### 종료하기
**_RootChainManager_** 컨트랙트의 exit 함수는 **_EtherPredicate_**에서 토큰을 잠금 해제하고 다시 받기 위해 호출되어야 합니다. 이 함수는 레코딩 트랜잭션을 증명하는 단일 바이트 인수를 사용합니다. 이 함수를 호출하기 전에 제출될 소각 트랜잭션을 포함하는 체크포인트를 기다리십시오. 증명은 다음 필드를 RLP 인코딩하여 생성됩니다.

1. headerNumber - 소각 트랜잭션을 포함하는 체크포인트 헤더 블록 번호
2. blockProof – (하위체인의) 블록 헤더가 제출된 머클 루트의 잎이라는 증명
3. blockNumber - 하위 체인의 소각 트랜잭션을 포함하는 블록 번호
4. blockTime - 트랜잭션 블록 시간 소각하기
5. txRoot - 블록의 트랜잭션 루트
6. receiveRoot - 블록의 영수 루트
7. receipt - 소각 트랜잭션의 영수증
8. ReceiptProof - 소각 영수증의 머클 증명
9. branchMask - 머클 패트리샤 트리에서 수신 경로를 나타내는 32비트
10. receiveLogIndex - 영수증에서 읽을 로그 인덱스

증명을 수동으로 생성하는 것은 까다로울 수 있으므로 Polygon Edge를 사용하는 것이 좋습니다. 트랜잭션을 수동으로 보내려면, 옵션 개체에서 **_encodeAbi_**를 **_true_**로 전달하여 raw calldata를 얻을 수 있습니다.
```js
const exitCalldata = await maticPOSClient
  .exitERC20(burnTxHash, { from, encodeAbi: true })
```

이 호출 데이터를 **_RootChainManager_**로 보냅니다.
```js
await mainWeb3.eth.sendTransaction({
  from: userAddress,
  to: rootChainManagerAddress,
  data: exitCalldata.data
})
```
