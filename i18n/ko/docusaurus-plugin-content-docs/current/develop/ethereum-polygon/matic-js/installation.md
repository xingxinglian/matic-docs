---
id: installation
title: 설치
keywords:
  - pos client
  - erc20
  - withdrawExit
  - 폴리곤
  - sdk
description: maticjs으로 시작하기
---

maticjs 에는 두 부분이 있습니다-

1. 메인 라이브러리
2. 이더리움 라이브러리

### 메인 라이브러리

메인 라이브러리에는 핵심 로직이 있으며 다양한 API를 제공합니다. 사용자는 주로 이 라이브러리와 상호 작용합니다.

```
npm i @maticnetwork/maticjs
```

### 이더리움 라이브러리

이더리움 라이브러리를 사용하면 선호하는 모든 ether 라이브러리를 사용할 수 있습니다.  플러그인을 사용하여 maticjs에 주입됩니다.

matic.js는 두 가지 인기 있는 라이브러리를 지원합니다 -

1. [Web3.js](https://web3js.readthedocs.io/)
2. [Ethers](https://docs.ethers.io/)

#### Web3.js

```
npm install @maticnetwork/maticjs-web3
```

#### ethers

```
npm install @maticnetwork/maticjs-ethers
```
