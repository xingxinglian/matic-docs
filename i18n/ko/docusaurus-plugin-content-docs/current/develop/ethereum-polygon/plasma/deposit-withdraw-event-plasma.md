---
id: deposit-withdraw-event-plasma
title: 입금 및 체크포인트 이벤트 추적 - 플라즈마
sidebar_label: 입금 및 체크포인트 이벤트 추적
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

시작하려면 최신 [Plasma에 대한 Matic.js 문서](https://maticnetwork.github.io/matic.js/docs/plasma/) 를 확인하세요.

## 입금 이벤트

토큰이 이더리움에서 폴리곤으로 입금되면 상태 동기화 메커니즘이라는 프로세스가 작동하여 결국 폴리곤 체인에서 사용자를 위한 토큰을 발행합니다.  이 프로세스는 발생하는 데 약 5-7분이 걸리므로 좋은 사용자 경험을 만들기 위해 입금 이벤트를 듣는 것이 매우 중요합니다. 다음은 실시간 입금 이벤트를 추적하는 데 사용할 수 있는 예제 스크립트입니다.

### 웹 소켓 연결을 통한 실시간 입금 이벤트 추적

```jsx
const WebSocket = require("ws");
const _ = require("lodash");

// For Mumbai
const ws = new WebSocket("wss://ws-mumbai.matic.today/");
// For Polygon mainnet: wss://ws-mainnet.matic.network/

async function checkDepositStatus(user, token, childChain) {
  return new Promise((resolve, reject) => {
    ws.on("open", function open() {
      ws.send(
        `{"id": 1, "method": "eth_subscribe", "params": ["newDeposits", {"Contract": "${childChain}"}]}`
      );
      ws.on("message", function incoming(data) {
        var txData = _.get(JSON.parse(data), "params.result.Data", "");
        var userAddress = txData.substring(0, 64).replace(/^0+/, "0x");
        var contractAddress = txData.substring(65, 128).replace(/^0+/, "0x");

        if (
          user &&
          user.toLowerCase() === userAddress.toLowerCase() &&
          token &&
          token.toLowerCase() === contractAddress.toLowerCase()
        ) {
          console.log(data);
          resolve(true); // eslint-disable-line
        }
      });

      ws.on("close", () => {
        reject(false);
      });

      ws.on("error", () => {
        reject(false);
      });
    });
  });
}

// Param1 - user address
// Param2 - contract address on main chain
// Param3 - child chain address (0xD9c7C4ED4B66858301D0cb28Cc88bf655Fe34861 for mainnet)
checkDepositStatus(
  "0xFd71Dc9721d9ddCF0480A582927c3dCd42f3064C",
  "0x499d11E0b6eAC7c0593d8Fb292DCBbF815Fb29Ae",
  "0x1EDd419627Ef40736ec4f8ceffdE671a30803c5e"
)
  .then((res) => {
    console.log(res);
  })
  .catch((err) => {
    console.log(err);
  });
```

### 블록체인을 쿼리하여 입금 완료 이력 확인.

이 스크립트는 하위 체인에서 특정 입금이 완료되었는지 여부를 확인하는 데 사용할 수 있습니다. 메인 체인과 하위 체인은 두 체인의 전역 카운터 변수 값을 계속 증가시킵니다. [StateSender](https://github.com/maticnetwork/contracts/blob/develop/contracts/root/stateSyncer/StateSender.sol#L38) 컨트랙트는 카운터 값이 있는 이벤트를 내보냅니다. 하위 체인의 카운터 값은  [StateReceiver](https://github.com/maticnetwork/genesis-contracts/blob/master/contracts/StateReceiver.sol#L12) 컨트랙트에서 쿼리할 수 있습니다. 하위 체인의 카운터 값이 메인 체인과 같거나 크면 입금이 성공적으로 완료된 것으로 간주할 수 있습니다.

```js
let Web3 = require("web3");

// For mainnet, use Ethereum RPC
const provider = new Web3.providers.HttpProvider(
  "https://goerli.infura.io/v3/API-KEY"
);
const web3 = new Web3(provider);

// For mainnet, use the Polygon mainnet RPC: <Sign up for a dedicated free RPC URL at https://rpc.maticvigil.com/ or other hosted node providers.>
const child_provider = new Web3.providers.HttpProvider(
  "<insert Mumbai testnet RPC URL>" //get a free RPC URL at https://rpc.maticvigil.com/ or other hosted node providers.
);

const child_web3 = new Web3(child_provider);

const contractInstance = new child_web3.eth.Contract(
  [
    {
      constant: true,
      inputs: [],
      name: "lastStateId",
      outputs: [
        {
          internalType: "uint256",
          name: "",
          type: "uint256",
        },
      ],
      payable: false,
      stateMutability: "view",
      type: "function",
    },
  ],
  "0x0000000000000000000000000000000000001001"
);

async function depositCompleted(txHash) {
  let tx = await web3.eth.getTransactionReceipt(txHash);
  let child_counter = await contractInstance.methods.lastStateId().call();
  let root_counter = web3.utils.hexToNumberString(tx.logs[3].topics[1]);
  return child_counter >= root_counter;
}

// Param 1 - Deposit transaction hash
depositCompleted(
  "0x29d901174acd42d4651654a502073f3c876ff85b7887b2e2634d00848f6c982e"
)
  .then((res) => {
    console.log(res);
  })
  .catch((err) => {
    console.log(err);
  });
```

## 체크포인트 이벤트

### 실시간 체크포인트 상태 추적

폴리곤 체인에서 발생하는 모든 트랜잭션은 유효성 검사기에 의해 빈번한 시간 간격으로 이더리움 체인에 체크 포인트됩니다. 이 시간은 뭄바이에서 ~10분, 폴리곤 메인넷에서 ~30분입니다. 체크포인트는 이더리움 체인에 배포된 RootChain 컨트랙트이라는 컨트랙트에서 발생합니다. 다음 스크립트를 사용하여 실시간 체크포인트 포함 이벤트를 수신할 수 있습니다.

```jsx
const Web3 = require("web3");

// Ethereum provider
const provider = new Web3.providers.WebsocketProvider(
  "wss://goerli.infura.io/ws/v3/api-key"
);

const web3 = new Web3(provider);

// Sign up for a free dedicated RPC URL at https://rpc.maticvigil.com/ or other hosted node providers.
const chil_provider = new Web3.providers.HttpProvider(
  "<insert Mumbai testnet RPC URL>"
);
const child_web3 = new Web3(chil_provider);

async function checkInclusion(txHash, rootChainAddress) {
  let txDetails = await child_web3.eth.getTransactionReceipt(txHash);

  block = txDetails.blockNumber;
  return new Promise(async (resolve, reject) => {
    web3.eth.subscribe(
      "logs",
      {
        address: rootChainAddress,
      },
      async (error, result) => {
        if (error) {
          reject(error);
        }

        console.log(result);
        if (result.data) {
          let transaction = web3.eth.abi.decodeParameters(
            ["uint256", "uint256", "bytes32"],
            result.data
          );
          if (block <= transaction["1"]) {
            resolve(result);
          }
        }
      }
    );
  });
}

// Param1 - Burn transaction hash on child chain
// Param2 - RootChainProxy Address on root chain (0x86E4Dc95c7FBdBf52e33D563BbDB00823894C287 for mainnet)
checkInclusion(
  "0x9d1e61d9daaa12fcd00fcf332e1c06fd8253a949b4f2a4741c964454a67ea943",
  "0x2890ba17efe978480615e330ecb65333b880928e"
)
  .then((res) => {
    console.log(res);
    provider.disconnect();
  })
  .catch((err) => {
    console.log(err);
  });
```

### 블록체인을 쿼리하여 기록 체크포인트 포함 확인.

이는 다음 API를 사용하여 확인할 수 있습니다. 하위 체인에 있는 소각 트랜잭션의 블록 번호는 이 GET API에 매개변수로 주어져야 합니다.

```
// Testnet
https://apis.matic.network/api/v1/mumbai/block-included/block-number
// Mainnet
https://apis.matic.network/api/v1/matic/block-included/block-number
```
