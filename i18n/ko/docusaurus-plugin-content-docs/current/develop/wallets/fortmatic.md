---
id: fortmatic
title: Fortmatic
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

Fortmatic SDK를 사용하면 이미 web3와 통합된 dApp이 있든 처음부터 시작하든 상관없이 앱을 이더리움 블록체인과 쉽게 통합할 수 있습니다. Fortmatic은 귀하와 귀하의 앱 사용자 모두에게 부드럽고 즐거운 경험을 제공합니다.

**npm 패키지 설치하기**

```bash
$ npm i --save fortmatic@latest
```

**예시**

```js title="example.js"
import Fortmatic from 'fortmatic';
import Web3 from 'web3';

const customNodeOptions = {
    rpcUrl: 'https://rpc-mumbai.matic.today', // your own node url
    chainId: 80001 // chainId of your own node
}

// Setting network to localhost blockchain
const fm = new Fortmatic('YOUR_TEST_API_KEY', customNodeOptions);
window.web3 = new Web3(fm.getProvider());
```