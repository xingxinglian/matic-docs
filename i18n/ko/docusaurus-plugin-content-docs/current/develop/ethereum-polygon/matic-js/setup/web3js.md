---
id: web3
title: 'Web3js 설정'
keywords:
  - pos client
  - erc20
  - withdrawExit
  - 폴리곤
  - sdk
description: 'maticjs으로 시작하기'
---

# Web3.js

[web3.js](https://web3js.readthedocs.io/) 는 HTTP, IPC 또는 WebSocket을 사용하여 로컬 또는 원격 이더리움 노드와 상호 작용할 수 있는 라이브러리 모음입니다.

## web3.js 설정하기

web3.js 지원은 matic.js용 플러그인으로 별도 패키지를 통해 제공됩니다.

### 설치

```
npm install @maticnetwork/maticjs-web3

```

### 설정

```
import { use } from '@maticnetwork/maticjs'
import { Web3ClientPlugin } from '@maticnetwork/maticjs-web3'

// install web3 plugin
use(Web3ClientPlugin)
```

web3를 사용하여 `POSClient`를 생성하는 예를 살펴보겠습니다 -

```
import { POSClient,use } from "@maticnetwork/maticjs"
import { Web3ClientPlugin } from '@maticnetwork/maticjs-web3'
import HDWalletProvider from "@truffle/hdwallet-provider"

// install web3 plugin
use(Web3ClientPlugin);

const posClient = new POSClient();
await posClient.init({
    network: 'testnet',
    version: 'mumbai',
    parent: {
      provider: new HDWalletProvider(privateKey, mainRPC),
      defaultConfig: {
        from : fromAddress
      }
    },
    child: {
      provider: new HDWalletProvider(privateKey, childRPC),
      defaultConfig: {
        from : fromAddress
      }
    }
});

```

## 예시

다른 경우에 대한 예시는 [web3 plugin repo](https://github.com/maticnetwork/maticjs-web3)에서 확인할 수 있습니다.
