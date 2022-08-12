---
id: torus
title: Torus
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

Torus는 DApp을 위한 사용자 친화적이고 안전한 비보관 키 관리 시스템입니다. 우리는 주류 사용자들에게 탈중앙화 생태계로 가는 관문을 제공하는 데 중점을 두고 있습니다.

**유형**: 비 보관l/HD <br/> **프라이빗 키 저장소**: 사용자의 로컬 브라우저 저장소 / torus의 서버에 암호화되어 저장 <br/> **이더리움 원장과의 통신**: Infura <br/> **프라이빗 키 인코등**: 니모닉(Mnemonic)/Social-Auth-login <br/>

애플리케이션 요구 사항에 따라 Torus는 Torus Wallet을 통해 통합되거나 DirectAuth를 통해 Torus 네트워크와 직접 상호 작용하여 통합될 수 있습니다. 자세한 내용은 Torus 설명서를 참조하십시오: https://docs.tor.us/getting-started

## 1. Torus 지갑 통합

Torus지갑 빠른 시작: https://docs.tor.us/torus-wallet/quick-start

애플리케이션이 이미 메타마스크/다른 web3 공급자와 호환되는 경우 Torus Wallet을 통합하면 공급자가 동일한 web3 인터페이스를 래핑할 수 있습니다. 당신은 npm 패키지나 IPFS, 또는 jsdeliver나 unpkg를 통해 설치할 수 있습니다. 자세한 내용은 지갑 통합에 대한 Torus 문서를 참조하십시오: https://docs.tor.us/getting-started#torus-wallet-integration

**npm 패키지 설치하기**

```bash
npm i @toruslabs/torus-embed
```

**예시**

```js title="torus-example.js"
import Torus from "@toruslabs/torus-embed";
import Web3 from "web3";

const torus = new Torus({
  buttonPosition: "top-left" // default: bottom-left
});
await torus.init({
  buildEnv: "production", // default: production
  enableLogging: true, // default: false
  network: {
    host: "mumbai", // default: mainnet
    chainId: 80001, // default: 1
    networkName: "Mumbai Test Network" // default: Main Ethereum Network
  },
  showTorusButton: false // default: true
});
await torus.login(); // await torus.ethereum.enable()
const web3 = new Web3(torus.provider);
```

## 2. DirectAuth 통합

로그인에서 모든 상호 작용에 이르기까지 자신의 UX를 제어하려는 경우 DirectAuth가 적합합니다. 구축 중인 플랫폼에 따라 SDK 중 하나를 통해 통합할 수 있습니다. 자세한 내용은 Torus direct auth integration을 참조하십시오: https://docs.tor.us/direct-auth/quick-start
