---
id: walletconnect
title: Wallet Connect
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

Wallet Connect는 DApp과 Wallet 간의 통신 링크를 생성하기 위해 구축된 지갑이 아닌 개방형 프로토콜입니다. 이 프로토콜을 지원하는 지갑과 애플리케이션은 두 피어 간의 공유 키를 통해 보안 링크를 활성화합니다. 표준 WalletConnect URI가 있는 QR 코드를 표시하는 DApp에 의해 연결이 시작되고 지갑 애플리케이션이 연결 요청을 승인하면 연결이 설정됩니다. 자금 이체에 대한 추가 요청은 지갑 애플리케이션 자체에서 확인됩니다.

## 1. Web3 설정하기

사용자의 폴리곤 지갑에 연결하도록 DApp을 설정하기 위해 Wallet Connect의 provider를 사용하여 폴리곤에 직접 연결할 수 있습니다. DApp에 다음을 설치합니다:

```bash
npm install --save @maticnetwork/walletconnect-provider
```

Matic 통합을 위한 matic.js 설치합니다:

```bash
$ npm install @maticnetwork/maticjs
```
그리고 앱에 다음 코드를 추가합니다,

```js
import WalletConnectProvider from "@maticnetwork/walletconnect-provider"

import Web3 from "web3"
import Matic from "maticjs"
```

다음으로 Wallet Connect의 개체를 통해 Polygon 및 Ropsten provider를 설정합니다:

```javascript
const maticProvider = new WalletConnectProvider(
  {
    host: `https://rpc-mumbai.matic.today`,
    callbacks: {
      onConnect: console.log('connected'),
      onDisconnect: console.log('disconnected!')
    }
  }
)

const ropstenProvider = new WalletConnectProvider({
  host: `https://ropsten.infura.io/v3/70645f042c3a409599c60f96f6dd9fbc`,
  callbacks: {
    onConnect: console.log('connected'),
    onDisconnect: console.log('disconnected')
  }
})
```
Web3 개체를 인스턴스화하기 위해 위의 두 provider 개체를 만들었습니다:


```js
const maticWeb3 = new Web3(maticProvider)
const ropstenWeb3 = new Web3(ropstenProvider)
```


## 2. 컨트랙트 인스턴스화하기

web3 객체가 있으면 컨트랙트를 인스턴스화하는 데 메타마스크에 대해 수행한 것과 동일한 단계가 포함됩니다.

> 다시 말하지만, 컨트랙트 ABI와 주소가 이미 있다고 가정합니다. :)

```js
const myContractInstance = new this.maticWeb3.eth.Contract(myContractAbi, myContractAddress)
```

## 3. 함수 호출하기

위에서 논의한 것처럼 이더리움에는 블록체인과의 상호 작용에 따라 두 가지 유형의 함수가 있습니다. 데이터를 읽을 때 `call()`, 데이터를 쓸 때 `send( )`.

### `call()` 함수 호출하기

이제 데이터를 읽는 데 서명이 필요하지 않으므로 프로세스는 위에서 설명한 것과 동일합니다:

```js
this.myContractInstance.methods
  .myMethod(myParams)
  .call()
  .then (
  // do stuff with returned values
  )
```
### `send()`함수 호출하기

블록체인에 쓰기 위해서는 서명이 필요하므로 지갑 연결을 지원하는 지갑의 사용자에게 트랜잭션 서명을 요청합니다.

여기에는 두 단계가 포함됩니다.
1. 트랜잭션 만들기
2. 트랜잭션에 대한 서명 받기
3. 서명된 트랜잭션 보내기


```js
const tx = {
  from: this.account,
  to: myContractAddress,
  gas: 800000,
  data: this.myContractInstance.methods.myMethod(myParams).encodeABI(),
}
```


위의 코드는 서명을 위해 사용자의 지갑으로 전송되는 트랜잭션 객체를 생성합니다:


```js
maticWeb3.eth.signTransaction(tx)
  .then((result) =>{
    maticWeb3.eth.sendSignedTransaction(result)
    .then((receipt) => 
    console.log (receipt)
  )
})
```

`signTransaction()` 함수는 사용자에게 서명을 요청하고 `sendSignedTransaction()`은 서명된 트랜잭션을 보냅니다(성공 시 트랜잭션 영수증 반환).

> 참고: 이동안 내내 프라이빗 키는 사용자의 지갑에 있으며 앱은 어떠한 방식으로도 액세스하지 않습니다. :) :)
