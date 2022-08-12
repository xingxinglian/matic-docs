---
id: getting-started
title: 폴리곤 엣지로 시작하기
sidebar_label: 폴리곤 엣지 인스턴스화하기
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

시작하려면, [최신Matic.js 문서](/docs/develop/ethereum-polygon/matic-js/get-started)를 확인하십시오.

## 빠른 요약

matic.js SDK는 폴리곤의 모든 컴퓨팅 성능을 여러분의 손끝에 바로 배치합니다. 너무 많은 노력을 기울이지 않고도 승인, 입금 및 출금을 허용하는 맞춤형 함수들을 제공합니다. 우리가 이를 엔지니어링한 이유는 플랫폼에서 즉각적인 가치를 얻을 수 있도록 하기 위함이었습니다.

## 설치
SDK를 통해 폴리곤의 강력한 기능을 사용하는 첫 번째 단계는 NPM 설치를 수행하는 것입니다. [여기](https://www.npmjs.com/package/@maticnetwork/maticjs)를 찾아가십시오.

```bash
npm install @maticnetwork/maticjs
npm install @maticnetwork/maticjs-web3
npm install @maticnetwork/maticjs-ethers
```

## 활용
SDK에 액세스하려면 다음을 사용하여 애플리케이션에서 가져옵니다.
```js
import { use } from '@maticnetwork/maticjs'
import { Web3ClientPlugin } from '@maticnetwork/maticjs-web3'
import HDWalletProvider from "@truffle/hdwallet-provider"

// install web3 plugin
use(Web3ClientPlugin)
```

공급자는 요구 사항에 따라 Metamask 공급자, HDWalletProvider 등과 같은 RPC URL 또는 web3 기반 공급자가 될 수 있습니다.

자세한 내용은  [PoS에 대한 Matic.js 문서](https://maticnetwork.github.io/matic.js/docs/pos/)를 참조하십시오.

```js
// for mumbai testnet
const getPOSClient = (network = 'testnet', version = 'mumbai') => {
  const posClient = new POSClient();

await posClient.init({
    network: <network name>,  // 'testnet' or 'mainnet'
    version: <network version>, // 'mumbai' or 'v1'
    parent: {
      provider: <parent provider>,
      defaultConfig: {
            from: <from address>
      }
    },
    child: {
      provider: <child provider>,
      defaultConfig: {
            from: <from address>
      }
    }
});
```
