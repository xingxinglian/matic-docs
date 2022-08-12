---
id: deposit-withdraw-event-pos
title: 입금 및 체크포인트 이벤트 추적 - PoS
sidebar_label: 입금 및 체크포인트 이벤트 추적
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## 빠른 요약

문서의 이 섹션에서는 폴리곤 생태계 내에서 수행되는 트랜잭션의 간격과 속도를 추적하고 모니터링하는 방법을 다룹니다. 네트워크로 입금(PoS 브리지로 완료한 경우)하는 것은 일반적으로 평균 5-7분이 소요되지만, 사용자가 실시간 진행 보고서를 확인하려는 경우를 보아 왔습니다. 개발자로서, 당신은 사용자에게 즉각적인 피드백을 제공하여 앱의 UX를 보강할 수도 있습니다. . 이 모든 경우에 이 섹션을 살펴보십시오. 우리는 당신이 필요로 하는 것을 정확히 갖고 있습니다.

## 입금 이벤트

토큰이 이더리움에서 폴리곤으로 입금되면 상태 동기화 메커니즘이라는 프로세스가 작동하여 결국 폴리곤 체인에서 사용자를 위한 토큰을 발행합니다. 이 프로세스는 발생하는 데 약 5-7분이 걸리므로 좋은 사용자 경험을 만들기 위해 입금 이벤트에 주위를 기울이는 것은 매우 중요합니다. 다음은 실시간 입금 이벤트를 추적하는 데 사용할 수 있는 예제 스크립트입니다.

### 웹 소켓 연결을 통한 실시간 입금 이벤트 추적

```jsx
const WebSocket = require("ws");
const Web3 = require("web3");

// For Mumbai
const ws = new WebSocket("wss://ws-mumbai.matic.today/");
// For Polygon mainnet: wss://ws-mainnet.matic.network/
const web3 = new Web3();
const abiCoder = web3.eth.abi;

async function checkDepositStatus(
  userAccount,
  rootToken,
  depositAmount,
  childChainManagerProxy
) {
  return new Promise((resolve, reject) => {
    ws.on("open", () => {
      ws.send(
        `{"id": 1, "method": "eth_subscribe", "params": ["newDeposits", {"Contract": "${childChainManagerProxy}"}]}`
      );

      ws.on("message", (msg) => {
        const parsedMsg = JSON.parse(msg);
        if (
          parsedMsg &&
          parsedMsg.params &&
          parsedMsg.params.result &&
          parsedMsg.params.result.Data
        ) {
          const fullData = parsedMsg.params.result.Data;
          const { 0: syncType, 1: syncData } = abiCoder.decodeParameters(
            ["bytes32", "bytes"],
            fullData
          );

          // check if sync is of deposit type (keccak256("DEPOSIT"))
          const depositType =
            "0x87a7811f4bfedea3d341ad165680ae306b01aaeacc205d227629cf157dd9f821";
          if (syncType.toLowerCase() === depositType.toLowerCase()) {
            const {
              0: userAddress,
              1: rootTokenAddress,
              2: depositData,
            } = abiCoder.decodeParameters(
              ["address", "address", "bytes"],
              syncData
            );

            // depositData can be further decoded to get amount, tokenId etc. based on token type
            // For ERC20 tokens
            const { 0: amount } = abiCoder.decodeParameters(
              ["uint256"],
              depositData
            );
            if (
              userAddress.toLowerCase() === userAccount.toLowerCase() &&
              rootToken.toLowerCase() === rootTokenAddress.toLowerCase() &&
              depositAmount === amount
            ) {
              resolve(true);
            }
          }
        }
      });

      ws.on("error", () => {
        reject(false);
      });

      ws.on("close", () => {
        reject(false);
      });
    });
  });
}

// Param1 - user address
// Param2 - contract address on main chain
// Param3 - amount deposited on main chain
// Param4 - child chain manager proxy address (0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa for mainnet)
checkDepositStatus(
  "0xFd71Dc9721d9ddCF0480A582927c3dCd42f3064C",
  "0x47195A03fC3Fc2881D084e8Dc03bD19BE8474E46",
  "1000000000000000000",
  "0xb5505a6d998549090530911180f38aC5130101c6"
)
  .then((res) => {
    console.log(res);
  })
  .catch((err) => {
    console.log(err);
  });
```

### 블록체인을 쿼리함으로써 입금완료 이력 체크

이 스크립트는 하위 체인에서 특정 입금이 완료되었는지 여부를 확인하는 데 사용할 수 있습니다. 메인 체인과 하위 체인은 두 체인의 전역 카운터 변수 값을 계속 증가시킵니다. [StateSender](https://github.com/maticnetwork/contracts/blob/develop/contracts/root/stateSyncer/StateSender.sol#L38) 컨트랙트는 카운터 값이 있는 이벤트를 내보냅니다. 하위 체인의 카운터 값은  [StateReceiver](https://github.com/maticnetwork/genesis-contracts/blob/master/contracts/StateReceiver.sol#L12) 컨트랙트에서 쿼리할 수 있습니다. 하위 체인의 카운터 값이 메인 체인과 같거나 크면 입금이 성공적으로 완료된 것으로 간주할 수 있습니다.

```jsx
let Web3 = require("web3");

// For mainnet, use Ethereum RPC
const provider = new Web3.providers.HttpProvider(
  "https://goerli.infura.io/v3/API-KEY"
);
const web3 = new Web3(provider);

// For mainnet, use the Polygon mainnet RPC: <Sign up for a dedicated free RPC URL at https://rpc.maticvigil.com/ or other hosted node providers.>
const child_provider = new Web3.providers.HttpProvider(
  "<insert Mumbai testnet RPC URL>" //Get a free RPC URL from https://rpc.maticvigil.com/ or other hosted node providers.
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

폴리곤 체인에서 발생하는 모든 트랜잭션은 유효성 검사기에 의해 빈번한 시간 간격으로 이더리움 체인에 체크 포인트됩니다. 이 시간은 뭄바이에서 ~10분, 폴리곤 메인넷에서 ~30분입니다. 체크포인트는 이더리움 체인에 배포된 **RootChainContract**이라는 컨트랙트에서 발생합니다. 다음 스크립트를 사용하여 실시간 체크포인트 포함 이벤트를 수신할 수 있습니다.

```js
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

// txHash - transaction hash on Polygon
// rootChainAddress - root chain proxy address on Ethereum
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

### 블록체인 쿼리를 통한 이력 체크포인트 포함 확인

이는 다음 API를 사용하여 확인할 수 있습니다. 하위 체인에 있는 소각 트랜잭션의 블록 번호는 이 GET API에 매개변수로 주어져야 합니다.

```js
// Testnet
https://apis.matic.network/api/v1/mumbai/block-included/block-number
// Mainnet
https://apis.matic.network/api/v1/matic/block-included/block-number
```
