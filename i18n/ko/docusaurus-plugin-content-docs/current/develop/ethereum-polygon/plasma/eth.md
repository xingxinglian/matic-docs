---
id: eth
title: ETH 입출금 가이드
sidebar_label: ETH
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

### 높은 수준의 작업흐름

#### **ETH 입금하기(1단계 프로세스)**

**deposit** 함수는 토큰이 폴리곤 컨트랙트에 입금되고 폴리곤 네트워크에서 사용할 수 있는 곳에서 호출됩니다.

#### **ETH 전송하기**

폴리곤에 자금이 있으면 해당 자금을 사용하여 다른 사람에게 즉시 보낼 수 있습니다.

#### **ETH 출금하기 (3단계 프로세스)**

1. 자금 인출은 폴리곤에서 시작됩니다. 30분의 체크포인트 간격(테스트넷의 경우 ~10분 대기)이 설정되어 있으며, 여기서 폴리곤 블록 레이어의 모든 블록은 마지막 체크포인트 이후로 검증됩니다.
2. 체크포인트가 메인체인 ERC20 컨트랙트에 제출되면 동등한 가치의 NFT Exit(ERC721) 토큰이 생성됩니다.
3. 인출된 자금은 프로세스 종료 절차를 사용하여 메인체인 컨트랙트에서 ERC20 계정으로 다시 청구할 수 있습니다.

## 세부정보 설정

---

### Matic SDK 구성하기

Matic SDK (**_3.0.0)_** 설치하기

```bash
npm i @maticnetwork/maticjs-plasma
```

### util.js

Maticjs 클라이언트 시작하기

```js
// const use = require('@maticnetwork/maticjs').use
const { Web3ClientPlugin } = require('@maticnetwork/maticjs-web3')
const { PlasmaClient } = require('@maticnetwork/maticjs-plasma')
const { use } = require('@maticnetwork/maticjs')
const HDWalletProvider = require('@truffle/hdwallet-provider')
const config = require('./config')

// install web3 plugin
use(Web3ClientPlugin)

const privateKey = config.user1.privateKey
const from = config.user1.address

async function getPlasmaClient (network = 'testnet', version = 'mumbai') {
  try {
    const plasmaClient = new PlasmaClient()
    return plasmaClient.init({
      network: network,
      version: version,
      parent: {
        provider: new HDWalletProvider(privateKey, config.parent.rpc),
        defaultConfig: {
          from
        }
      },
      child: {
        provider: new HDWalletProvider(privateKey, config.child.rpc),
        defaultConfig: {
          from
        }
      }
    })
  } catch (error) {
    console.error('error unable to initiate plasmaClient', error)
  }
}
```

### process.env

루트 디렉토리에 process.env라는 새 파일을 만듭니다.

```bash
USER1_FROM =
USER1_PRIVATE_KEY =
USER2_ADDRESS =
ROOT_RPC =
MATIC_RPC =
```

---

## deposit.js

**deposit**: 입금은 depositManagerContract 컨트랙트에서 **_depositEther_**를 호출하여 수행할 수 있습니다.

> 토큰은 사전에 매핑되고 전송 승인을 받아야 합니다.

이 호출을 수행하려면 **_depositEther_** 메소드를 사용하십시오.

```js
const { getPOSClient, from } = require('../../utils');

const execute = async () => {
  const client = await getPOSClient();
  const result = await client.depositEther(100, from);

  const txHash = await result.getTransactionHash();
  const receipt = await result.getReceipt();

};

execute().then(() => {
}).catch(err => {
  console.error("err", err);
}).finally(_ => {
  process.exit(0);
})
```

> 참고: 이더리움에서 폴리곤으로의 입금은 상태 동기화 메커니즘을 사용하여 발생하며 약 5-7분 정도 걸립니다. 이 시간 간격을 두고 기다린 후 web3.js/matic.js 라이브러리나 메타마스크를 이용하여 잔고를 확인하는 것을 권장합니다. 탐색기는 하위 체인에서 하나 이상의 자산 전송이 발생한 경우에만 잔고를 표시합니다. 이 [링크](/docs/develop/ethereum-polygon/plasma/deposit-withdraw-event-plasma)는 입금 이벤트를 추적하는 방법을 설명합니다.

## transfer.js

폴리곤 네트워크의 이더는 랩이더WETH(ERC20 Token)입니다.

```js
const { getPlasmaClient, from, plasma, to } = require('../utils')

const amount = '1000000000' // amount in wei
const token = plasma.child.erc20

async function execute () {
  try {
    const plasmaClient = await getPlasmaClient()
    const erc20Token = plasmaClient.erc20(token)
    const result = await erc20Token.transfer(amount, to, { gasPrice: 1000000000 })
    const txHash = await result.getTransactionHash()
  } catch (error) {
    console.log(error)
  }
}

execute().then(() => {
}).catch(err => {
  console.error('err', err)
}).finally(_ => {
  process.exit(0)
})
```

## Withdraw

### 1. 소각

사용자는 **_getERC20TokenContract_** 하위 토큰 컨트랙트의 **_withdraw_** 함수를 호출할 수 있습니다. 이 함수는 토큰을 소각할 것입니다. Polygon Plasma 클라이언트는 이 호출을 수행하기 위해 **_withdrawStart_** 메소드를 노출합니다.

```js
const { getPlasmaClient, from, plasma } = require('../utils')

const amount = '1000000000000000' // amount in wei
const token = plasma.child.erc20
async function execute () {
  const plasmaClient = await getPlasmaClient()
  const erc20Token = plasmaClient.erc20(token)
  const result = await erc20Token.withdrawStart(amount)

  const txHash = await result.getTransactionHash()
  const receipt = await result.getReceipt()

}

execute().then(() => {
}).catch(err => {
  console.error('err', err)
}).finally(_ => {
  process.exit(0)
```

### 2. confirm-withdraw.js


사용자는 **_erc20Predicate_** 컨트랙트의 **_startExitWithBurntTokens_** 함수를 호출할 수 있습니다. 이 함수는 토큰을 소각할 것입니다. Polygon Plasma 클라이언트는 이 호출을 수행하기 위해 **_withdrawConfirm_** 메소드를 노출합니다. 이 함수는 체크포인트가 메인 체인에 포함된 후에만 호출할 수 있습니다. 이 [가이드](/docs/develop/ethereum-polygon/plasma/deposit-withdraw-event-plasma#checkpoint-events)에 따라 체크포인트 포함을 추적할 수 있습니다.


```js
//Wait for ~10 mins for Mumbai testnet or ~30mins for Ethereum Mainnet till the checkpoint is submitted for burned transaction, then run the confirm withdraw
const { getPlasmaClient, from, plasma } = require('../utils')

async function execute () {
  const plasmaClient = await getPlasmaClient()
  const erc20Token = plasmaClient.erc20(plasma.parent.erc20, true)
  const result = await erc20Token.withdrawConfirm(<burn tx hash>)

  const txHash = await result.getTransactionHash()
  const txReceipt = await result.getReceipt()
}

execute().then(_ => {
  process.exit(0)
})
```

### 3. Process Exit

사용자는 **_withdrawManager_** 컨트랙트의 **_processExits_** 함수를 호출하고 소각 증명을 제출해야 합니다. 유효한 증명 토큰을 제출하면 사용자에게 전송됩니다. Polygon Plasma 클라이언트는 이 호출을 수행하기 위해 **_withdrawExit_** 메서드를 노출합니다.

```js
const { getPlasmaClient, from, plasma } = require('../utils')

async function execute () {
  const plasmaClient = await getPlasmaClient()
  const erc20Token = plasmaClient.erc20(plasma.parent.erc20, true);
  const result = await erc20Token.withdrawExit();

  const txHash = await result.getTransactionHash()
  const txReceipt = await result.getReceipt()
  console.log(txReceipt)
}

execute().then(_ => {
  process.exit(0)
})
```

_참고: 폴리곤에서 이더리움 체인에 대해 5분 간격으로 발생하는 모든 트랜잭션을 나타내는 체크포인트가 메인체인 이더리움 컨트랙트에 제출됩니다._
