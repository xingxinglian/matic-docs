---
id: dagger
title: Dagger
sidebar_label: Dagger - 단일 App
description: Matic에서 다음 블록체인앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Dagger는 Ethereum Blockchain에서 실시간 업데이트를 얻는 가장 좋은 방법입니다. DApp 및 백엔드 시스템이 웹 소켓 또는 소켓을 통해 실시간으로 트랜잭션, 토큰 전송, 영수증 및 로그와 같은 이더리움 블록체인 이벤트를 얻을 수 있는 방법을 제공합니다.

안정적이고 확장 가능한 실시간 이벤트를 위한 인프라를 유지 관리합니다. `@maticnetwork/dagger`는 NodeJS로 작성된 Dagger 프로젝트의 소비자 라이브러리입니다. Dagger 서버를 사용하여 이더리움 네트워크에서 실시간 업데이트를 가져옵니다.

## 설치

```sh
# Using Yarn
yarn add @maticnetwork/dagger

# Using NPM
npm install @maticnetwork/dagger --save
```

## 네트워크

### 이더리움 네트워크

#### 메인넷

```sh
Websocket: wss://mainnet.dagger.matic.network
Socket: mqtts://mainnet.dagger.matic.network (You can also use `ssl://` protocol)
```

#### Kovan

```sh
Websocket: wss://kovan.dagger.matic.network
Socket: mqtts://kovan.dagger.matic.network (You can also use `ssl://` protocol)
```

#### Ropsten

```sh
Websocket: wss://ropsten.dagger.matic.network
Socket: mqtts://ropsten.dagger.matic.network (You can also use `ssl://` protocol)
```

#### Goerli

```sh
Websocket: wss://goerli.dagger.matic.network
Socket: mqtts://goerli.dagger.matic.network (You can also use `ssl://` protocol)
```

### Matic 네트워크

#### 메인넷

```sh
Websocket: wss://matic-mainnet.dagger.matic.network
Socket: mqtts://matic-mainnet.dagger.matic.network (You can also use `ssl://` protocol)
```

#### 뭄바이 테스트넷

```sh
Websocket: wss://mumbai-dagger.matic.today
Socket: mqtts://mumbai-dagger.matic.today (You can also use `ssl://` protocol)
```

## 예시

- 먼저 _npm_ 프로젝트를 만듭시다.

```bash
npm init -y
touch index.js
```

- 이제 `index.js`에 다음 코드 스니펫을 넣을 수 있습니다.

```javascript
const Dagger = require('@maticnetwork/dagger')

// connect to correct dagger server, for receiving network specific events
//
// you can also use socket based connection
const dagger = new Dagger("wss://mainnet.dagger.matic.network")

// get new block as soon as it gets created
dagger.on('latest:block.number', result => {
  console.log(`New block created: ${result}`)
})
```

- `index.js` 실행 & 새 블록이 생성되는 즉시 블록 번호를 받기 시작합니다.

```bash
node index.js
```

## API

### new Dagger(url)

dagger 객체 만들기

- `url`는 dagger 서버의 주소입니다. 사용가능한 모든 url 값을 위해 [네트워크 섹션](#network)을 확인합니다.

예시:

```js
const dagger = new Dagger(<url>)
```

### dagger.on(event, fn)

토픽 구독

- `event`는 구독할 `String` 토픽입니다. `event` 와일드카드 문자가 지원됩니다(`+` - 단일 레벨의 경우 및 `#` - 다중 레벨의 경우).
- `fn` - `function (data, removed)` fn은 이벤트가 발생할 때 실행됩니다:
  - `data` 이벤트의 데이터
  - `removed` 플래그: 재구성으로 인해 블록체인에서 데이터가 제거되었는지 여부를 나타냄.

예시:

```js
dagger.on('latest:block.number', (res, flag) => { console.log(res, flag) })
```

### dagger.once(event, fn)

[on](#daggeronevent-fn)과 동일하지만 한 번만 실행됩니다.

예시:

```js
dagger.once('latest:block.number', (res, flag) => { console.log(res, flag) })
```

### dagger.off(event, fn)

토픽 구독취소

- `event`는 구독을 취소할 `String` 토픽입니다.
- `fn` - `function (data, removed)`

예시:

```js
dagger.off('latest:block.number', (res, flag) => { console.log(res, flag) })
```

### dagger.of(room)

dagger로 공간을 만듭니다. `room`은 두 값 중 하나여야 합니다.
  - `latest`
  - `confirmed`

`room` 개체에는 다음과 같은 메소드가 있습니다
  - `on` same as dagger `on`
  - `once` same as dagger `once`
  - `off` same as dagger `off`

```js
const latestRoom = dagger.of('latest')
const confirmedRoom = dagger.of('confirmed')
```

### dagger.end([force])

dagger를 닫고 다음 옵션을 수락합니다.

- `force`: true로 전달하면 dagger가 즉시 닫힙니다. 이 매개변수는 선택사항입니다.

```js
dagger.end({force: true}) // immediate closing
```

### dagger.contract(web3Contract)

Dagger를 지원하는 web3 계약 래퍼를 만듭니다.

- 먼저 web3 컨트랙트 개체를 만듭니다.

```javascript
// web3 contract
const web3Contract = new web3.eth.Contract(abi, address)
```

- 이제 우리는 그것에 dagger 컨트랙트 래퍼를 만들 것입니다.

```javascript
// dagger contract
const contract = dagger.contract(web3Contract)
```

- 계약 이벤트를 필터링할 시간

```javascript
const filter = contract.events.Transfer({
  filter: { from: "0x123456..." },
  room: "latest"
})
```

- 컨트랙트 이벤트 보기

```javascript
// watch
filter.watch((data, removed) => { console.log(data, removed) })

// or watch only once
filter.watchOnce((data, removed) => { console.log(data, removed) })
```

- 이벤트 보기 중지

```js
// stop watching
filter.stopWatching();
```

## 이벤트

모든 이벤트는 {`latest`, `confirmed`}가 포함된 공간을 갖고 있습니다.
  - `latest` : 이벤트는 체인에 블록이 포함된 후 즉시 발생합니다.
  - `confirmed` : 이벤트는 12번의 컨펌 후에 시작됩니다.

DApp의 UI에 업데이트를 표시하려면 `latest` 이벤트를 사용하세요. UI/UX를 개선하고 사용자 친화적으로 만드는 데 도움이 됩니다.

서버 또는 UI에서 되돌릴 수 없는 작업에 대해 `confirmed` 이벤트를 사용합니다. 이메일 전송, 알림 또는 사용자가 하나의 트랜잭션이 확인된 후 UI에서 후속 작업을 수행하도록 허용합니다.

### 네트워크 이벤트

| 이더리움 이벤트                                       | 언제?                                                       | `제거된` 플래그 |
| ---------------------------------------------- | --------------------------------------------------------- | --------- |
| block                                          | 생성된 모든 새 블록을 위해                                           | Yes       |
| block.number                                   | 생성된 모든 새 블록 번호를 위해                                        |           |
| block.hash                                     | 생성된 모든 새 블록 해시를 위해                                        | Yes       |
| block/`number`                                 | 미래의 특정 블록이 체인에 포함될 때                                      | Yes       |
| addr/`address`/tx                              | `address`에 대한 모든 신규 거래 시                                  | Yes       |
| addr/`address`/tx/out                          | `address`에 대한 모든 새로운 발신 트랜잭션시                             | Yes       |
| addr/`address`/tx/in                           | `address`에 대한 모든 신규 수신 트랜잭션시                              | Yes       |
| tx/`txId`                                      | 주어진 `txId`가 블록에 포함될 때                                     | Yes       |
| tx/`txId`/success                              | `txId`에 대한 tx 상태가 성공(블록에 포함됨)인 때                          | Yes       |
| tx/`txId`/fail                                 | `txId`에 대한 tx가 실패할 때(블록에 포함됨)                             | Yes       |
| tx/`txId`/receipt                              | `txId`에 대한 영수증이 생성될 때(블록에 포함됨)                            | Yes       |
| addr/`contractAddress`/deployed                | 새로운 `contractAddress`가 블록에 포함될 때                          | Yes       |
| log/`contractAddress`                          | `contractAddress`에 대한 새 로그가 생성될 때                         | Yes       |
| log/`contractAddress`/filter/`topic1`/`topic2` | `contractAddress`에 대해 `topic1` 및 `topic2`가 있는 새 로그가 생성된 때 | Yes       |

### Dagger 이벤트

| Dagger 이벤트        | 언제?          | 인수             |
| ----------------- | ------------ | -------------- |
| connection.status | 연결 상태가 변경될 때 | value: Boolean |


모든 이벤트는 다음과 같이 시작해야 합니다:

#### block

생성된 모든 새 블록을 위해

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:block", result => {
  console.log("Current block : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:block", result => {
  console.log("Confirmed block : ", result)
})
```

</TabItem>
</Tabs>

#### block.number

모든 새 블록 번호를 위해

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:block.number", result => {
  console.log("Current block number : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:block.number", result => {
  console.log("Confirmed block number : ", result)
})
```

</TabItem>
</Tabs>

#### block.hash

모든 새 블록 해시를 위해

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:block.hash", result => {
  console.log("Current block hash : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:block.hash", result => {
  console.log("Confirmed block hash : ", result)
})
```

</TabItem>
</Tabs>

#### block/{number}

특정 블록이 **X**일 때 향후 체인에 포함됩니다.

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:block/X", result => {
  console.log("Included in chain : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:block/X", result => {
  console.log("Included in chain : ", result)
})
```

</TabItem>
</Tabs>

#### addr/{address}/tx

`address`에 대한 모든 신규 거래 시

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:addr/{address}/tx", result => {
  console.log("New Transaction : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:addr/{address}/tx", result => {
  console.log("New Transaction : ", result)
})
```

</TabItem>
</Tabs>

#### addr/{address}/tx/{dir}

`dir`은 {`in`, `out`}을 포함한 트랜잭션 방향입니다. `address`는 모든 주소에 대한 알림을 받기 위해 생략할 수 있습니다.

<Tabs
  defaultValue="in"
  values={[
    { label: 'incoming', value: 'in', },
 { label: 'outgoing', value: 'out', },
 { label: 'wild card', value: 'all', },
 ]
}>
<TabItem value="in">

`address`에 대한 모든 새로운 수신 트랜잭션에 대해

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:addr/{address}/tx/in", result => {
  console.log("New Incoming Transaction : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:addr/{address}/tx/in", result => {
  console.log("New Incoming Transaction : ", result)
})
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="out">

`address`에 대한 모든 새로운 발신 트랜잭션에서

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:addr/{address}/tx/out", result => {
  console.log("New Outgoing Transaction : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:addr/{address}/tx/out", result => {
  console.log("New Outgoing Transaction : ", result)
})
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="all">

모든 수신 및 발신 트랜잭션에 대한 알림을 받기 위해 '주소' 대신 와일드카드 표기법을 사용합니다.

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:addr/+/tx/in", result => {
  console.log("New Incoming Transaction : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:addr/+/tx/in", result => {
  console.log("New Incoming Transaction : ", result)
})
```

</TabItem>
</Tabs>

</TabItem>
</Tabs>

#### tx/{txId}/{status}

`status`은 {`success`, `fail`, `receipt`}를 포함한 `txId`의 상태입니다. 빈 상태로 유지될 수도 있습니다. 즉, `txId`가 블록에 포함될 때 트리거되는 `tx/{txId}`가 됩니다.

<Tabs
  defaultValue="any"
  values={[
    { label: 'any', value: 'any', },
 { label: 'success', value: 'success', },
 { label: 'fail', value: 'fail', },
 { label: 'receipt', value: 'receipt', },
 ]
}>
<TabItem value="any">

블록에 포함된 `txId`가 주어졌을 때

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:tx/{txId}", result => { console.log(result) })
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:tx/{txId}", result => { console.log(result) })
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="success">

`txId`에 대한 tx 상태가 성공(블록에 포함)인 경우

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:tx/{txId}/success", result => { console.log(result) })
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:tx/{txId}/success", result => { console.log(result) })
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="fail">

`txId`에 대한 tx가 실패할 때(블록에 포함됨)

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:tx/{txId}/fail", result => { console.log(result) })
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:tx/{txId}/fail", result => { console.log(result) })
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="receipt">

`txId`에 대한 영수증이 생성될 때(블록에 포함됨)

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:tx/{txId}/receipt", result => { console.log(result) })
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:tx/{txId}/receipt", result => { console.log(result) })
```

</TabItem>
</Tabs>

</TabItem>
</Tabs>

#### log/{contractAddress}

`contractAddress`에 대한 로그 생성 시

<Tabs
  defaultValue="latest"
  values={[
    { label: 'latest', value: 'latest', },
 { label: 'confirmed', value: 'confirmed', },
 ]
}>
<TabItem value="latest">

```javascript
dagger.on("latest:log/{contractAddress}", result => {
  console.log("New Log : ", result)
})
```

</TabItem>
<TabItem value="confirmed">

```javascript
dagger.on("confirmed:log/{contractAddress}", result => {
  console.log("New Log : ", result)
})
```

</TabItem>
</Tabs>

#### log/{contractAddress}/filter/{topic0}/{topic1}/{topic2}

`topic0`, `topic1` & `contractAddress`에 대해 생성된 `topic2`

```javascript
// Triggers when 1 GNT (Golem token) get transferred to Golem multisig wallet
dagger.on('latest:log/0xa74476443119a942de498590fe1f2454d7d4ac0d/filter/0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef/filter/+/0x7da82c7ab4771ff031b66538d2fb9b0b047f6cf9/#', console.log)

// Triggers when any amount of GNT (Golem token) get sent from Golem multisig wallet
dagger.on('latest:log/0xa74476443119a942de498590fe1f2454d7d4ac0d/filter/0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef/0x7da82c7ab4771ff031b66538d2fb9b0b047f6cf9/#', ...)

// Listen for every Golem token transfer (notice `#` at the end)
dagger.on('latest:log/0xa74476443119a942de498590fe1f2454d7d4ac0d/filter/0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef/#', ...)
```

> 이벤트 이름은 대소문자를 구분합니다. `address`, `txId` 및 `topics`는 소문자여야 합니다.

> 참고: 이벤트에도 와일드카드를 사용할 수 있습니다. 와일드카드에는 `+`(단일)와 `#`(여러 개)의 두 가지 유형이 있습니다. 필요한 것보다 더 많은 데이터를 가져오고 DApp에 데이터를 폭격할 수 있으므로 주의하여 사용하십시오.



## 테스트 Dagger 서버

이 라이브러리는 로컬 컴퓨터의 테스트 dagger 서버인 `woodendagger` 실행 파일로 구성됩니다. 따라서 TestRPC로 테스트할 수 있습니다.

프로덕션에서 `woodendagger`를 사용하지 마십시오. 그것은 개발 목적만을 위한 것입니다. `removed` 플래그를 지원하지 않습니다.

```bash
$ woodendagger --url=https://mainnet.infura.io # or http://localhost:8545 for local json-rpc

# If you want to start dagger server on different ports,
# sockport: socket port for backend connection over TCP
# wsport: websocket port for frontend connection over websocket
$ woodendagger --url=http://localhost:8545 --sockport=1883 --wsport=1884

# To connect from dagger:
const dagger = new Dagger('mqtt://localhost:1883')
```

## 지원

질문, 피드백 또는 기능 요청이 있는 경우 [Telegram](https://t.me/maticnetwork)으로 언제든지 문의해 주세요.

## 라이선스

MIT
