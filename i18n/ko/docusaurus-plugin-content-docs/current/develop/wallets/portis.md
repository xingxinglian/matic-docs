---
id: portis
title: Portis
description: Matic에서 다음 블록체인앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

Portis는 쉬운 사용자 온보딩을 염두에 두고 구축된 웹 기반 지갑입니다. DApp에 통합되고 사용자를 위한 로컬 지갑이 없는 경험을 생성하는 자바스크립트 SDK와 함께 제공됩니다. 또한 지갑 설정, 트랜잭션 및 가스 수수료를 처리합니다. 메타마스크와 마찬가지로 비보관입니다 - 사용자는 키를 제어하고, Portis는 키를 안전하게 저장합니다. 그러나 메타마스크와 달리 브라우저가 아닌 응용 프로그램에 통합됩니다. 사용자는 로그인 ID 및 암호와 연결된 키를 가지고 있습니다.

**Type**: Non-custodial/HD <br/> **Private Key Storage**: Encrypted and stored on portis’ servers <br/> **Communication to Ethereum Ledger**: Developer defined <br/> **Private key encoding**: Mnemonic<br/>

### 1. Web3 설정하기

DApp에 다음을 설치하십시오:
```js
npm install --save @portis/web3
```

그리고 DApp ID를 얻기 위해 Portis에 DApp을 등록합니다:
> [Portis Dashboard](https://dashboard.portis.io/)

`portis` 및 `web3` 객체 가져오기:

```js
import Portis from '@portis/web3';
import Web3 from 'web3';
```
Portis 생성자는 첫 번째 인수를 DApp ID(이전 단계에서 가져옴)로 사용하고 두 번째 인수를 연결하려는 네트워크로 사용합니다. 이것은 문자열 또는 객체일 수 있습니다.
```js
const portis = new Portis('YOUR_DAPP_ID', 'maticTestnet');
const web3 = new Web3(portis.provider);
```
### 2. 계정 설정하기

web3의 설치 및 인스턴스화에 성공하면 다음은 연결된 계정을 성공적으로 반환해야 합니다:
```js
this.web3.eth.getAccounts()
.then((accounts) => {
  this.account = accounts[0];
})
```
### 3. 컨트랙트 인스턴스화하기

위에서 논의한 것처럼 컨트랙트의 인스턴스화는 동일하게 유지됩니다:
```js
const myContractInstance = new this.web3.eth.Contract(myContractAbi, myContractAddress)
```
### 4. 함수 호출하기

함수 호출은 위에서 설명한 것과 동일하게 유지됩니다: #### `call()`함수 호출하기
```js
this.myContractInstance.methods.myMethod(myParams)
.call()
.then (
  // do stuff with returned values
)
```
### `send()` 함수 호출하기
```js
this.myContractInstance.methods.myMethod(myParams)
.send({
  from: this.account,gasPrice: 0
})
.then ((receipt) => {
  // returns a transaction receipt
})
```