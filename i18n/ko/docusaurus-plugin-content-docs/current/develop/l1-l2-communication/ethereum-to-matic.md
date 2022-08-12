---
id: ethereum-to-matic
title: 이더리움에서 폴리곤으로 데이터 전송하기
description: 컨트랙트를 통해 이더리움에서 폴리곤으로 상태와 데이터 전송하기
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

폴리곤 EVM 체인에서 이더리움 데이터를 기본적으로 읽는 메커니즘은 '상태 동기화'입니다. 즉, 이 메커니즘은 Ethereum 체인에서 Polygon 체인으로 임의의 데이터를 전송할 수 있습니다. 이를 가능하게 하는 절차는 다음과 같습니다. Heimdall 계층의 유효성 검사기가 특정 이벤트를 수신 대기 중입니다. — 발신자 컨트랙트에서 `StateSynced`, 이벤트가 선택되는 즉시 이벤트에서 전달된 `데이터`가 수신자 컨트랙트에 기록됩니다. 더 많은 정보는 [여기](/docs/maintain/validator/core-components/state-sync-mechanism)를 읽어보십시오.

발신자와 수신자 컨트랙트는 이더리움에 매핑되어야 합니다 - [StateSender.sol](https://github.com/maticnetwork/contracts/blob/release-betaV2/contracts/root/stateSyncer/StateSender.sol)은 각 발신자와 수신자를 알고 있어야 합니다. 매핑을 완료하려면 [여기](https://mapper.polygon.technology/)에서 매핑을 요청하세요.

---

다음 연습에서는 Goerli(이더리움 테스트넷)에 Sender 컨트랙트를 배포하고 뭄바이(폴리곤의 테스트넷)에 Receiver 컨트랙트를 배포한 다음 노드 스크립트에서 web3 호출을 통해 Sender에서 데이터를 보내고 Receiver에서 데이터를 읽습니다.

### 1. Sender 컨트랙트 배포하기

Sender 컨트랙트의 유일한 목적은 Heimdall이 수신하는 StateSynced 이벤트인 StateSender 컨트랙트(Matic의 상태 동기화 컨트랙트)에서 [syncState](https://github.com/maticnetwork/contracts/blob/e999579e9dc898ab6e66ddcb49ee84c2543a9658/contracts/root/stateSyncer/StateSender.sol#L33) 함수를 호출할 수 있도록 하는 것입니다.

배포 위치:

Goerli에서 `0xEAa852323826C71cd7920C3b4c007184234c3945`

이더리움 메인넷에서 `0x28e4F3a7f651294B9564800b2D01f35189A5bFbE`

이 함수를 호출할 수 있도록 먼저 계약에 인터페이스를 포함시킵니다:

```jsx
// Sender.sol

pragma solidity ^0.5.11;

contract IStateSender {
  function syncState(address receiver, bytes calldata data) external;
  function register(address sender, address receiver) public;
}

...
```

다음으로 폴리곤에 전달하고 싶은 데이터를 받아 syncState를 호출하는 사용자 정의 함수를 작성해 보겠습니다.

```jsx
function sendState(bytes calldata data) external {
    states = states + 1 ;
    IStateSender(stateSenderContract).syncState(receiver, data);
}
```

위의 함수에서 `stateSenderContract`는 `Sender`를 배포할 네트워크의 StateSender 주소입니다. (예: Goerli의 경우 `0xEAa852323826C71cd7920C3b4c007184234c3945`를 사용합니다.) `수신자`는 여기에서 보내는 데이터를 수신하는 계약입니다.

생성자를 사용하여 변수를 전달하는 것이 좋지만 이 데모의 목적을 위해 다음 두 주소를 하드코딩합니다.

다음은 Sender.sol의 모습입니다.

```jsx
// sender.sol

pragma solidity ^0.5.11;

contract IStateSender {
  function syncState(address receiver, bytes calldata data) external;
  function register(address sender, address receiver) public;
}

contract sender {
  address public stateSenderContract = 0xEAa852323826C71cd7920C3b4c007184234c3945;
  address public receiver = 0x83bB46B64b311c89bEF813A534291e155459579e;

  uint public states = 0;

  function sendState(bytes calldata data) external {
    states = states + 1 ;
    IStateSender(stateSenderContract).syncState(receiver, data);
  }

}
```

Sender 컨트랙트를 통해 전송된 상태의 수를 추적하기 위해 간단한 `state` 카운터를 사용하고 있습니다.

Remix를 사용하여 계약을 배포하고 주소와 ABI를 기록해 둡니다.

### 2. Receiver 컨트랙트 배포하기

수신자 계약은 `StateSynced` 이벤트가 발생할 때 유효성 검사기에 의해 호출되는 컨트랙트입니다. Validator는 수신자 컨트랙트에서 `onStateReceive` 함수를 호출하여 데이터를 제출합니다. 이를 구현하기 위해 먼저 [StateReceiver](https://github.com/maticnetwork/contracts/blob/release-betaV2/contracts/child/bor/StateReceiver.sol) 인터페이스를 가져오고 사용자 정의 로직을 작성하여 onStateReceive 내부에서 전송된 데이터를 해석합니다.

다음은 Receiver.sol의 모습입니다.

```jsx
// receiver.sol

pragma solidity ^0.5.11;

// IStateReceiver represents interface to receive state
interface IStateReceiver {
  function onStateReceive(uint256 stateId, bytes calldata data) external;
}

contract receiver {

  uint public lastStateId;
  bytes public lastChildData;

  function onStateReceive(uint256 stateId, bytes calldata data) external {
    lastStateId = stateId;
    lastChildData = data;
    }

}
```

이 함수는 단순히 마지막으로 받은 상태 ID와 데이터를 변수에 할당합니다. [StateId](https://github.com/maticnetwork/contracts/blob/239a91045622ddcf9ebec2cec81fdc6daa3a33e3/contracts/root/stateSyncer/StateSender.sol#L36)는 전송된 상태(단순 카운터)에 대한 단순 고유 참조입니다.

Polygon의 테스트넷에 Receiver.sol을 배포하고 주소와 ABI를 기록해 둡니다.

### 3. 발신자와 수신자 매핑하기

발신자와 수신자에 대해 이미 배포된 주소(위에서 언급함)를 사용하거나 사용자 지정 컨트랙트를 배포하고 여기에서 매핑을 요청할 수 있습니다: [https://mapper.polygon.technology/](https://mapper.polygon.technology/)

### 4. 데이터 송수신

이제 컨트랙트가 완료되고 매핑이 완료되었으므로 임의의 16진수 바이트를 보내고 폴리곤에서 수신하고 데이터를 해석하는 간단한 노드 스크립트를 작성합니다!

**4.1 스크립트 설정**

우리는 먼저 web3 객체, 지갑을 초기화하여 거래와 컨트랙트를 할 것입니다.

```jsx
// test.js

const Web3 = require('web3')
const Network = require("@maticnetwork/meta/network")

const network = new Network ('testnet', 'mumbai')

const main = new Web3(network.Main.RPC)
const matic = new Web3 (network.Matic.RPC)

let privateKey = `0x...` // add or import your private key

matic.eth.accounts.wallet.add(privateKey)
main.eth.accounts.wallet.add(privateKey)

let receiverAddress = `<RECEIVER_CONTRACT_ADDRESS>`
let receiverABI = `` // insert or import ABI
let senderAddress = `<SENDER_CONTRACT_ADDRESS>`
let senderABI = `` // insert of import the ABI

let sender = new main.eth.Contract(JSON.parse(senderABI), senderAddress)
let receiver = new matic.eth.Contract(JSON.parse(receiverABI), receiverAddress)

```

우리는 RPC에 @maticnetwork/meta 패키지를 사용하고 있습니다. 이 패키지는 스크립트를 실행하기 위한 요구 사항이 아닙니다.

`matic` 및 `주요` 개체는 각각 폴리곤 및 Ropsten의 RPC로 초기화된 web3 개체를 참조합니다.

`sender` 및 `receiver` 객체는 1단계와 2단계에서 배포한 Sender.sol 및 Receiver.sol의 컨트랙트 객체를 참조합니다.

**4.2 데이터 전송하기**

다음으로 데이터의 바이트열을 생성하고 Sender 컨트랙트를 통해 전송하도록 함수를 설정해 보겠습니다.

```jsx
// data to sync
function getData(string) {
  let data = matic.utils.asciiToHex(string);
  return data
}

// send data via sender
async function sendData (data) {
  let r = await sender.methods
    .sendState (getData(data))
    .send({
      from: main.eth.accounts.wallet[0].address,
      gas: 8000000
    })
  console.log('sent data from root, ', r.transactionHash)
}
```

`getData`를 호출하면 ASCII 문자열(예: `Hello World !`)이 바이트 문자열(예: `0x48656c6c6f20576f726c642021`)로 변환됩니다. `sendData` 함수가 `데이터`(ascii 문자열)를 가져오는 동안 `getData`를 호출하고 바이트열을 보낸 사람 컨트랙트에 전달합니다.

**4.3 데이터 수신하기**

다음으로 Receiver.sol에서 수신된 데이터를 확인합니다.

상태 동기화가 실행되는 데 7~8분 정도 걸립니다.

다음 함수를 추가하여  Sender에서 보낸 상태의 숫자(a)와 Receiver에서 마지막으로 받은 상태(b)를 확인합니다.

```jsx
// check `states` variable on sender
async function checkSender () {
  let r = await sender.methods
    .states()
    .call()
  console.log('number of states sent from sender: ', r)
}

// check last received data on receiver
async function checkReceiver () {
  let r = await receiver.methods
    .lastStateId()
    .call()
  let s = await receiver.methods
    .lastChildData()
    .call()
  console.log('last state id: ', r, 'and last data: ', s)
  console.log('interpreted data: ', getString(s))
}
```

`checkReceiver` 함수는 컨트랙트에서 정의한 변수를 호출하기만 하면 됩니다. 이 변수는 Validator가 컨트랙트에서 `onStateReceive`를 호출하는 즉시 설정됩니다. `getString` 함수는 단순히 바이트열을 해석합니다(다시 ASCII로 변환).

```jsx
function getString(data) {
  let string = matic.utils.hexToAscii(data);
  return string
}
```

마지막으로 우리는 함수를 실행하는 메소드를 작성할 것입니다:

```jsx
async function test() {
    await sendData ('Sending a state sync! :) ')
    await checkSender ()
    await checkReceiver ()
}
```

**4.4 모든 것을 함께 넣습니다!**

테스트 스크립트는 다음과 같습니다.

```jsx
// test.js

const Web3 = require('web3')
const Network = require("@maticnetwork/meta/network")

const network = new Network ('testnet', 'mumbai')

const main = new Web3(network.Main.RPC)
const matic = new Web3 (network.Matic.RPC)

let privateKey = `0x...`
matic.eth.accounts.wallet.add(privateKey)
main.eth.accounts.wallet.add(privateKey)

let receiverAddress = `<RECEIVER_CONTRACT_ADDRESS>`
let receiverABI = ``
let senderAddress = `<SENDER_CONTRACT_ADDRESS>`
let senderABI = ``

let sender = new main.eth.Contract(JSON.parse(senderABI), senderAddress)
let receiver = new matic.eth.Contract(JSON.parse(receiverABI), receiverAddress)

// data to sync
function getData(string) {
  let data = matic.utils.asciiToHex(string);
  return data
}

function getString(data) {
  let string = matic.utils.hexToAscii(data);
  return string
}

// console.log(getData('Sending a state sync! :) '))

async function sendData (data) {
  let r = await sender.methods
    .sendState (getData(data))
    .send({
      from: main.eth.accounts.wallet[0].address,
      gas: 8000000
    })
  console.log('sent data from root, ', r.transactionHash)
}

async function checkSender () {
  let r = await sender.methods
    .states()
    .call()
  console.log('number of states sent from sender: ', r)
}

async function checkReceiver () {
  let r = await receiver.methods
    .lastStateId()
    .call()
  let s = await receiver.methods
    .lastChildData()
    .call()
  console.log('last state id: ', r, 'and last data: ', s)
  console.log('interpreted data: ', getString(s))
}

async function test() {
    await sendData ('Hello World !')
    await checkSender ()
    // add a timeout here to allow time gap for the state to sync
    await checkReceiver ()
}

test()
```

**4.5 스크립트를 실행하자**

위 스크립트를 성공적으로 실행하면 다음과 같은 출력이 제공됩니다.

```bash
$ node test
> sent data from root 0x4f64ae4ab4d2b2d2dc82cdd9ddae73af026e5a9c46c086b13bd75e38009e5204
number of states sent from sender: 1
last state id: 453 and last data: 0x48656c6c6f20576f726c642021
interpreted data: Hello World ! 
```
