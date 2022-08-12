---
id: bandstandarddataset
title: Band 표준 데이터세트
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
  - band
  - oracle
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

## 소개

폴리곤을 기반으로 하는 개발자들은 이제 Band의 분산된 Oracle 인프라를 활용할 수 있습니다. Band의 Oracle을 통해 이제 다양한 암호화폐의 가격 데이터에 액세스하여 애플리케이션에 통합할 수 있습니다.

## 지원되는 토큰

현재 지원되는 심볼들의 목록은 [data.bandprotocol.com](http://data.bandprotcool.com)에서 찾을 수 있습니다. 앞으로 이 목록은 개발자 요구 사항과 커뮤니티 피드백을 기반으로 계속 확장될 것입니다.

### 가격 쌍

다음 방법은 기준 및 인용 심볼들이 데이터 세트에서 지원되는 한 기준/인용 토큰 쌍의 모든 조합으로 작동할 수 있습니다.

## 가격 쿼리

현재 개발자가 Band의 오라클에서 가격을 쿼리할 수 있는 두 가지 방법이 있습니다: 폴리곤에 대한 Band의 `StdReference` 스마트 컨트랙트와 [`bandchain.js`](https://www.npmjs.com/package/%40bandprotocol%2Fbandchain.js) JavaScript 도우미 라이브러리를 사용하는 것입니다.

### Solidity 스마트 컨트랙트

Band의 오라클에서 가격을 쿼리하려면 스마트 컨트랙트가 Band의 StdReference 컨트랙트, 특히 `getReferenceData` 및 `getReferenceDatabulk` 메서드를 참조해야 합니다.

`getReferenceData`는 두 개의 문자열을 입력으로 각각 기본 및 인용 기호로 사용합니다. 그런 다음 `StdReference` 컨트랙트에서 해당 두 토큰에 대한 최신 요금을 쿼리하고 아래와 같이 `ReferenceData` 구조체를 반환합니다.

```solidity
struct ReferenceData {
    uint256 rate; // base/quote exchange rate, multiplied by 1e18.
    uint256 lastUpdatedBase; // UNIX epoch of the last time when base price gets updated.
    uint256 lastUpdatedQuote; // UNIX epoch of the last time when quote price gets updated.
}
```

`getReferenceDataBulk`는 대신 `base`토큰 중 하나와 `quotes` 중 하나인 두 개의 목록을 사용합니다. 그런 다음 각 인덱스에서 각 기준/인용 쌍의 가격을 유사하게 쿼리하고 `ReferenceData` 구조체의 배열을 반환합니다.

예를 들어, `['BTC','BTC','ETH']` 및 `['USD','ETH','BNB']`로 `getReferenceDataBulk`를 호출하면 반환된 `ReferenceData` 배열에는 쌍에 대한 정보가 포함됩니다:

- `BTC/USD`
- `BTC/ETH`
- `ETH/BNB`


#### 컨트랙트 주소

| 블록체인     |                   컨트랙트 주소                    |
| -------- |:--------------------------------------------:|
| 폴리곤(테스트) | `0x56e2898e0ceff0d1222827759b56b28ad812f92f` |


#### 사용예

이 [컨트랙트](https://gist.github.com/tansawit/a66d460d4e896aa94a0790df299251db)는 Band의 `StdReference` 컨트랙트와 `getReferenceData` 함수를 사용하는 예를 보여줍니다.


### BandChain.JS

Band의 노드 도우미 라이브러리 [`bandchain.js`](https://www.npmjs.com/package/@bandprotocol/bandchain.js)도 유사한 `getReferenceData` 함수를 지원합니다. 이 함수는 결과를 쿼리할 토큰 쌍 목록인 하나의 인수를 취합니다. 그런 다음 해당 비율 값 목록을 반환합니다.


#### 사용 예

아래 코드는 함수의 사용 예를 보여줍니다.

```javascript=
const { Client } = require('@bandprotocol/bandchain.js');

// BandChain's REST Endpoint
const endpoint = 'https://rpc.bandchain.org';
const client = new Client(endpoint);

// This example demonstrates how to query price data from
// Band's standard dataset
async function exampleGetReferenceData() {
  const rate = await client.getReferenceData(['BTC/ETH','BAND/EUR']);
  return rate;
}

(async () => {
  console.log(await exampleGetReferenceData());
})();

```

해당 결과는 다음과 유사합니다.

```bash
$ node index.js
[ 
    { 
        pair: 'BTC/ETH',
        rate: 30.998744363906173,
        updatedAt: { base: 1615866954, quote: 1615866954 },
        requestID: { base: 2206590, quote: 2206590 } 
    },
    { 
        pair: 'BAND/EUR',
        rate: 10.566138918332376,
        updatedAt: { base: 1615866845, quote: 1615866911 },
        requestID: { base: 2206539, quote: 2206572 } 
    }
]
```

각 쌍에 대해 다음 정보가 반환됩니다:

- `pair`: 기준/인용 심볼 쌍의 문자열
- `rate`: 주어진 쌍의 결과 비율
- `updated`: 기준 및 인용 기호가 BandChain에서 마지막으로 업데이트된 타임스탬프입니다. `USD`의 경우 현재 타임스탬프가 됩니다.
- `rawRate`: 이 개체는 두 부분으로 구성됩니다.
  - `value` 는  실제 비율의 `BigInt` 값에  `10^decimals`를 곱한 값입니다.
  - `decimals` 는 `rawRate` 를 얻기 위해 `rate` 을 곱한 지수입니다.
