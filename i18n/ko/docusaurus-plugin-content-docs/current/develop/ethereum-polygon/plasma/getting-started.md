---
id: getting-started
title: 플라즈마 브리지
sidebar_label: 소개
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---
import useBaseUrl from '@docusaurus/useBaseUrl';

시작하려면 가장 최신의 [Plasma에 대한 Matic.js 문서](https://maticnetwork.github.io/matic.js/docs/plasma/)를 확인하세요.

브리지는 기본적으로 루트 체인에서 하위 체인으로 자산을 이동하는 데 도움이 되는 일련의 컨트랙트입니다. 이더리움과 폴리곤 간에 자산을 이동하는 데는 주로 두 가지 브리지가 있습니다. 첫 번째는 Plasma Bridge이고 두 번째는 **PoS Bridge** 또는 **Proof of Stake Bridge**라고 합니다. **Plasma Bridge**는 플라즈마 종료 메커니즘으로 인해 향상된 보안 보장을 제공합니다.

그러나 하위 토큰에는 특정 제한 사항이 있으며 Plasma Bridge에서 폴리곤으로부터 이더리움으로의 모든 종료/출금과 관련된 7일의 출금 기간이 있습니다. [PoS Bridge](/docs/develop/ethereum-polygon/pos/getting-started)는 더 유연하고 더 빠른 출금이 특징입니다.

이 자습서는 폴리곤 네트워크에서 Plasma Bridge와 상호 작용하는 가장 쉬운 방법인[ Matic JS](https://github.com/maticnetwork/matic.js)를 사용하여 Plasma Bridge를 이해하고 사용하는 단계별 프로세스에 대한 가이드 역할을 합니다.

## Plasma Bridge에서 자산의 흐름

이 튜토리얼에서 Polygon의 자산 전송의 흐름과 Matic.js를 사용하여 동일한 작업을 수행하는 방법을 보여줍니다:

<img src={useBaseUrl("img/matic/Matic-Workflow-2.jpg")} />

1. 사용자는 메인체인의 폴리곤 컨트랙트에 암호화 자산을 입금합니다.
2. 메인체인에 입금된 토큰이 확인되면 해당 토큰은 폴리곤 체인에 반영됩니다.
   - 이제 사용자는 적은 비용으로 원하는 사람에게 즉시 토큰을 전송할 수 있습니다. 폴리곤 체인은 더 빠른 블록(약 1초)을 가지고 있습니다. 그렇게 하면 전송이 거의 즉시 완료됩니다.
3. 사용자가 준비되면 메인체인에서 남은 토큰을 인출할 수 있습니다. 자금 인출은 플라즈마 사이드체인에서 시작됩니다. 5분의 체크포인트 간격이 설정되어 있으며, 여기서 폴리곤 블록 레이어의 모든 블록은 마지막 체크포인트 이후에 검증됩니다.
4. 체크포인트가 메인체인 이더리움 컨트랙트에 제출되면 동등한 가치의 Exit NFT(ERC721) 토큰이 생성됩니다.
5. 인출된 자금은 프로세스 종료 절차를 사용하여 메인체인 컨트랙트에서 이더리움 계정으로 다시 청구할 수 있습니다.
   - 사용자는 0x 또는 Dharma를 통해 빠른 종료를 얻을 수도 있습니다(곧 제공될 예정입니다!)

### 전제조건:

```
npm i @maticnetwork/maticjs-plasma

import { PlasmaClient } from "@maticnetwork/maticjs-plasma"

const plasmaClient = new PlasmaClient();

await plasmaClient.init({
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

### Görli Faucet

트랜잭션을 발생시키기 위해서, 자습서를 따르는 동안 사용할 테스트 계정에 약간의 이더도 필요합니다. Görli에 이더가 없는 경우 여기에 제공된 faucet 링크를 사용할 수 있습니다 —https://goerli-faucet.slock.it/.

### Polygon Faucet

이 튜토리얼 전체에서 우리는 Görli 네트워크에서 ERC20 토큰 `TEST`를 예로 사용할 것입니다. 이것은 TEST 토큰입니다. DApp에서 ERC20 토큰으로 대체할 수 있습니다. 폴리곤 네트워크에서 일부 테스트 `TEST` 토큰을 얻으려면 [Polygon Faucet](https://faucet.polygon.technology/)에 액세스할 수 있습니다.

> 참고: 입출금에 자신의 토큰을 사용하려면, '매핑된' 토큰을 갖고 있어야 합니다. 이는 본질적으로 메인 체인 및 사이드 체인의 컨트랙트가 맞춤 토큰을 '인식'하도록 만드는 것을 의미합니다. [여기](/docs/develop/ethereum-polygon/plasma/mapping-assets)에서 매핑 프로세스에 대해 자세히 알아보거나 [여기](/docs/develop/ethereum-polygon/submit-mapping-request)에서 매핑 요청을 제출할 수 있습니다.

### 메타마스크 지갑의 기본 설정(선택사항)

1. [지갑 만들기](/docs/develop/metamask/hello) : 지갑이 처음이라면 메타마스크 계정을 설정하십시오.
2. [폴리곤 테스트넷 구성하기](/docs/develop/metamask/config-polygon-on-metamask) : Polygon의 자금 흐름을 쉽게 시각화하려면 Metamask에 Polygon 테스트넷을 구성하는 것이 좋습니다.

   > 여기서는 시각화 목적으로만 메타마스크를 사용하고 있습니다. 폴리곤을 사용하기 위해 메타마스크를 사용할 필요는 전혀 없습니다.
3. [여러 계정 만들기](/docs/develop/metamask/multiple-accounts) : 튜토리얼을 시작하기 전에 3개의 이더리움 테스트 계정을 준비하세요.
4. [폴리곤에서 토큰 구성하기](/docs/develop/metamask/custom-tokens): Matic.js를 사용하여 폴리곤에 대한 자금 흐름을 쉽게 보기 위해 메타마스크에 토큰을 구성할 수 있습니다. 이 튜토리얼에서 예로 든 `TEST` 토큰은 계정 잔액을 쉽게 시각화할 수 있도록 메타마스크에서 구성할 수 있습니다. >이것은 **선택 사항**입니다. [web3](https://web3js.readthedocs.io/en/1.0/)를 사용하여 토큰 잔고 및 기타 변수를 매우 쉽게 쿼리할 수 있습니다.
