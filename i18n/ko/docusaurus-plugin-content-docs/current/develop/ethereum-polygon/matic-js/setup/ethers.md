---
id: ethers
title: 'Ethers 설정'
keywords:
  - pos client
  - erc20
  - withdrawExit
  - 폴리곤
  - sdk
description: 'maticjs으로 시작하기'
---

# Ether.js

[ethers.js](https://docs.ethers.io/) 라이브러리는 이더리움 블록체인 및 해당 생태계와 상호 작용하기 위한 완전하고 컴팩트한 라이브러리를 목표로 합니다.

## ether.js 설정하기

ether.js 지원은 matic.js용 플러그인으로 별도 패키지를 통해 제공됩니다.

### 설치

```
npm install @maticnetwork/maticjs-ethers

```

### 설정

```
import { use } from '@maticnetwork/maticjs'
import { Web3ClientPlugin } from '@maticnetwork/maticjs-ethers'

// install ethers plugin
use(Web3ClientPlugin)
```

ethers를 사용하여 `POSClient`를 생성하는 한 가지 예를 살펴보겠습니다 -

```
import { POSClient,use } from "@maticnetwork/maticjs"
import { Web3ClientPlugin } from '@maticnetwork/maticjs-ethers'
import { providers, Wallet } from "ethers";


// install web3 plugin
use(Web3ClientPlugin);

const parentProvider = new providers.JsonRpcProvider(rpc.parent);
const childProvider = new providers.JsonRpcProvider(rpc.child);

const posClient = new POSClient();
await posClient.init({
    network: 'testnet',
    version: 'mumbai',
    parent: {
      provider: new Wallet(privateKey, parentProvider),
      defaultConfig: {
        from : fromAddress
      }
    },
    child: {
      provider: new Wallet(privateKey, childProvider),
      defaultConfig: {
        from : fromAddress
      }
    }
});

```

## 예시

다른 경우에 대한 예제는 [ethers plugin repo](https://github.com/maticnetwork/maticjs-ethers)에서 확인할 수 있습니다.
