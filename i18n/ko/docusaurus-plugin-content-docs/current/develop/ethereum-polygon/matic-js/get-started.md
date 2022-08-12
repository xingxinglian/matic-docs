---
id: get-started
title: 시작하기
keywords:
  - maticjs
  - 소개
  - 컨트랙트
  - 폴리곤
  - sdk
description: maticjs으로 시작하기
---

`@matic.js`는 Matic 네트워크의 다양한 구성요소와 상호작용하는 데 도움이 되는 자바스크립트 라이브러리입니다.

이 시작하기 자습서에서는 POS 브리지를 설정하고 상호 작용하는 방법을 배웁니다.

## 설치

**npm 을 통해 maticjs 패키지를 설치합니다:**

```bash
npm install @maticnetwork/maticjs
```

**web3js 플러그인 설치하기**

```bash
npm install @maticnetwork/maticjs-web3
```

## 설정

```javascript
import { use } from '@maticnetwork/maticjs'
import { Web3ClientPlugin } from '@maticnetwork/maticjs-web3'

// install web3 plugin
use(Web3ClientPlugin)
```

위의 코드에서 우리는 `web3js`로 maticjs를 시작하고 있지만 마찬가지로 [ethers](/docs/develop/ethereum-polygon/matic-js/setup/ethers)로 시작할 수 있습니다

## Pos client

`POSClient`는 POS Bridge와 상호 작용할 수 있도록 도와줍니다.

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

`POSClient`가 시작된 후 `erc20`, `erc721` 등과 같은 필수 토큰 유형을 시작해야 합니다.

erc20을 시작합시다.

### erc20

**erc20 하위 토큰 만들기**

```
const erc20ChildToken = posClient.erc20(<token address>);
```

**erc20 상위 토큰 만들기**

```
const erc20ParentToken = posClient.erc20(<token address>, true);

```

erc20이 시작되면 `getBalance`, `approve`, `deposit` , `withdraw` 등과 같이 사용 가능한 다양한 메소드들을 호출할 수 있습니다.

몇 가지 API 예제를 살펴보겠습니다

#### get balance

```
const balance = await erc20ChildToken.getBalance(<userAddress>)
console.log('balance', balance)
```

#### approve

```
// approve amount 10 on parent token
const approveResult = await erc20ParentToken.approve(10);

// get transaction hash
const txHash = await approveResult.getTransactionHash();

// get transaction receipt
const txReceipt = await approveResult.getReceipt();
```


보시다시피, 간단한 API를 사용하여 maticjs는 maticjs 브리지와 매우 쉽게 상호 작용할 수 있습니다.  **멋진 것을 만드는 것을 시작해 봅시다**

### 몇 가지 중요한 링크

- [예시](https://github.com/maticnetwork/matic.js/tree/master/examples)
