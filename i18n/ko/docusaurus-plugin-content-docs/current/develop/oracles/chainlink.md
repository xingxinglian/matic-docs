---
id: chainlink
title: Chainlink
sidebar_label: Chainlink
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
  - chainlink
  - oracle
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

Chainlink를 사용하면 계약이 분산된 Oracle 네트워크를 통해 *어떤* 외부 데이터 소스에도 액세스할 수 있습니다. 귀하의 컨트랙트가 스포츠 결과, 최신 날씨 또는 기타 공개적으로 사용 가능한 데이터를 요구하는지 여부에 관계없이 Chainlink는 귀하의 컨트랙트가 이를 소비하는 데 필요한 도구를 제공합니다.

# 분산 데이터

Chainlinks의 가장 강력한 기능 중 하나는 이미 분산되고 집계되었으며 가장 인기 있는 대부분의 암호화폐에서 온체인 데이터를 소화할 준비가 되어 있습니다. 이를 [체인링크 데이터 피드](https://docs.chain.link/docs/using-chainlink-reference-contracts)라고 합니다.

다음은 Mumbai 테스트넷에서 USD로 MATIC의 최신 가격을 가져오는 컨트랙트의 실제 예입니다.

당신이 원하는 [어떠한 데이터 피드의 주소로](https://docs.chain.link/docs/matic-addresses#config) 바꾸기만 하면 가격 정보 요약을 시작할 수 있습니다.
```

pragma solidity ^0.6.7;

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Mumbai Testnet 
     * Aggregator: MATIC/USD
     * Address: 0xd0D5e3DB44DE05E9F294BB0a3bEEaF030DE24Ada
     */
    constructor() public {
        priceFeed = AggregatorV3Interface(0xd0D5e3DB44DE05E9F294BB0a3bEEaF030DE24Ada);
    }

    /**
     * Returns the latest price
     */
    function getLatestPrice() public view returns (int) {
        (
            uint80 roundID, 
            int price,
            uint startedAt,
            uint timeStamp,
            uint80 answeredInRound
        ) = priceFeed.latestRoundData();
        return price;
    }
}
```

# 요청 및 수신 주기

Chainlink의 요청 및 수신 주기를 사용하면 스마트 컨트랙트가 외부 API에 요청하고 응답을 사용할 수 있습니다. 이를 구현하려면 컨트랙트에서 두 가지 기능을 정의해야 합니다:

1. 하나는 데이터를 요청
2. 다른 하나는 응답 수신

데이터를 요청하기 위해 컨트랙트는 오라클에 제공하는 요청 객체를 빌드합니다. 오라클이 API에 접근하고 응답을 구문 분석하면 스마트 컨트랙트에 정의된 callback 함수를 사용하여 데이터를 다시 계약으로 보내려고 시도합니다.

# 용도

1. Chainlink 데이터 피드
   1. 이는 이미 온체인에 집계된 탈중앙화된 데이터 참조 지점이며 실제 세계에서 데이터를 얻는 가장 빠르고 쉽고 저렴한 방법입니다. 현재 가장 인기 있는 암호화폐 및 법정화폐 쌍을 지원합니다.
2. Chainlink VRF
   1. 난수가 암호학적으로 난수임을 보장하는 증명 가능한 난수를 얻으십시오.
3. Chainlink API 호출
   1. 기존 API와 함께 작동하도록 스마트 컨트랙트 구성하고 데이터를 가져오고 인터넷을 통해 요청을 보내는 등의 작업을 수행하도록 사용자 지정하는 방법.

데이터 피드로 작업하려면 Chainlink 문서의 [폴리곤 데이터 피드](https://docs.chain.link/docs/matic-addresses)를 사용하십시오.

Chainlink VRF로 작업하려면 [Chainlink 문서](https://docs.chain.link/docs/get-a-random-number)의[ Polygon VRF](https://docs.chain.link/docs/vrf-contracts) 주소를 사용하십시오.
# 코드 예시

외부 API와 상호 작용하려면 스마트 컨트랙트가 요청 처리를 쉽게 하도록 설계된 계약인 <a href="https://github.com/smartcontractkit/chainlink/blob/develop/evm-contracts/src/v0.6/ChainlinkClient.sol" target="_blank">ChainlinkClient</a>에서 상속해야 합니다. 컨트랙트에서 API 요청을 빌드하는 데 사용해야 하는 `Chainlink.Request`라는 구조체를 노출합니다.

요청은 오라클 주소, 작업 ID, 수수료, 어댑터 매개변수 및 콜백 함수 서명을 정의해야 합니다. 이 예에서 요청은 `requestEthereumPrice` 함수에서 빌드됩니다.

`fulfill`은 callback 함수로 정의됩니다.

```javascript
pragma solidity ^0.6.0;

import "@chainlink/contracts/src/v0.6/ChainlinkClient.sol";

contract APIConsumer is ChainlinkClient {

    uint256 public price;

    address private oracle;
    bytes32 private jobId;
    uint256 private fee;

    /**
     * Network: Polygon Mumbai Testnet
     * Oracle: 0x58bbdbfb6fca3129b91f0dbe372098123b38b5e9
     * Job ID: da20aae0e4c843f6949e5cb3f7cfe8c4
     * LINK address: 0x326C977E6efc84E512bB9C30f76E30c160eD06FB
     * Fee: 0.01 LINK
     */
    constructor() public {
        setChainlinkToken(0x326C977E6efc84E512bB9C30f76E30c160eD06FB);
        oracle = 0x58bbdbfb6fca3129b91f0dbe372098123b38b5e9;
        jobId = "da20aae0e4c843f6949e5cb3f7cfe8c4";
        fee = 10 ** 16; // 0.01 LINK
    }

    /**
     * Create a Chainlink request to retrieve API response, find the target price
     * data, then multiply by 100 (to remove decimal places from price).
     */
    function requestBTCCNYPrice() public returns (bytes32 requestId) 
    {
        Chainlink.Request memory request = buildChainlinkRequest(jobId, address(this), this.fulfill.selector);

        // Set the URL to perform the GET request on
        // NOTE: If this oracle gets more than 5 requests from this job at a time, it will not return. 
        request.add("get", "https://www.alphavantage.co/query?function=CURRENCY_EXCHANGE_RATE&from_currency=BTC&to_currency=CNY&apikey=demo");

       // Set the path to find the desired data in the API response, where the response format is:
       // {
       //     "Realtime Currency Exchange Rate": {
       //       "1. From_Currency Code": "BTC",
       //       "2. From_Currency Name": "Bitcoin",
       //       "3. To_Currency Code": "CNY",
       //       "4. To_Currency Name": "Chinese Yuan",
       //       "5. Exchange Rate": "207838.88814500",
       //       "6. Last Refreshed": "2021-01-26 11:11:07",
       //       "7. Time Zone": "UTC",
       //      "8. Bid Price": "207838.82343000",
       //       "9. Ask Price": "207838.88814500"
       //     }
       //     }
        string[] memory path = new string[](2);
        path[0] = "Realtime Currency Exchange Rate";
        path[1] = "5. Exchange Rate";
        request.addStringArray("path", path);

        // Multiply the result by 10000000000 to remove decimals
        request.addInt("times", 10000000000);

        // Sends the request
        return sendChainlinkRequestTo(oracle, request, fee);
    }

    /**
     * Receive the response in the form of uint256
     */ 
    function fulfill(bytes32 _requestId, uint256 _price) public recordChainlinkFulfillment(_requestId)
    {
        price = _price;
    }
}
```

# 메인넷 폴리곤LINK 토큰

이더리움 메인넷에서 메인넷 폴리곤 LINK 토큰을 얻으려면 2단계 프로세스를 따라야 합니다.

1. 플라즈마 또는 [PoS 브리지](https://wallet.polygon.technology/bridge)를 사용하여 LINK를 브리지합니다.
2. [Chainlink에서 배포한, Pegswap](https://pegswap.chain.link/)을 통해 LINK를 ERC677 버전으로 스왑합니다.

폴리곤 브리지는 LINK의 ERC20 버전을 가져오고 LINK는 ERC677이므로 이 스왑으로 업데이트해야 합니다.
# 주소

현재 Polygon Mumbai 테스트넷에 운영 중인 체인링크 오라클은 몇 개뿐입니다. 언제든지 직접 실행할 수 있으며 Chainlink Marketplace에 나열할 수 있습니다.

* Oracle: <a href="https://mumbai.polygonscan.com/address/0x58bbdbfb6fca3129b91f0dbe372098123b38b5e9/transactions" target="_blank">`0xb33D8A4e62236eA91F3a8fD7ab15A95B9B7eEc7D`</a>
* LINK: <a href="https://mumbai.polygonscan.com/address/0x70d1F773A9f81C852087B77F6Ae6d3032B02D2AB/transactions" target="_blank">`0x326C977E6efc84E512bB9C30f76E30c160eD06FB`</a>


Mumbai Testnet에서 LINK를 얻으려면 여기 <a href="https://faucet.polygon.technology/" target="_blank">faucet</a>으로 가십시오.

# 어떤 API가 지원됩니까?

Chainlink의 요청 및 수신 주기는 요청 매개변수가 정확하고 응답 형식을 알고 있는 한 모든 공개 API를 호출할 수 있을 만큼 충분히 유연합니다. 예를 들어, 가져오려는 URL의 응답 객체가 `{"USD":243.33}`과 같은 형식이라면, 경로는 단순합니다: `"USD"`.

API가 복잡한 JSON 개체로 응답하는 경우, "path" 매개변수는 중첩 개체에 대해 점으로 구분된 문자열을 사용하여 원하는 데이터를 검색할 위치를 지정해야 합니다. 예를 들어 다음 응답을 사용합니다:

```JSON
{
   "Prices":{
        "USD":243.33
    }
}
```

여기에는 다음 경로가 필요합니다: `"Prices.USD"` 문자열에 공백이 있거나 문자열이 상당히 긴 경우 위의 예에 표시된 구문을 사용할 수 있습니다. 여기에서 모두를 문자열 배열로써 전달합니다.

```
string[] memory path = new string[](2);
path[0] = "Prices";
path[1] = "USD";
request.addStringArray("path", path);
```

# Job ID는 무엇을 위한 것입니까?

예제에서 요청을 작성할 때 `jobId` 매개변수를 사용한다는 것을 눈치채셨을 것입니다. 작업은 Oracle이 실행하도록 구성된 일련의 명령으로 구성됩니다. 위의 [코드 예제](#code-example)에서 컨트랙트는 작업 ID가 `da20aae0e4c843f6949e5cb3f7cfe8c4`인 Oracle에 요청합니다. 이 특정 작업은 다음을 수행하도록 구성됩니다:

* GET 요청 만들기
* JSON 응답 구문 분석
* 값에 *x*를 곱함.
* 값을 `uint`로 변환
* 체인에 제출

이것이 우리의 계약이 URL, JSON 응답에서 원하는 데이터를 찾을 수 있는 경로 및 요청에 소요되는 시간을 추가하는 이유입니다; `request.add` 구문을 사용합니다. 이러한 지침은 Oracle에서 어댑터라고 하는 것으로 용이합니다.

**Oracle에 대한 모든 요청에는 특정 작업 ID가 포함되어야 합니다.**

다음은 폴리곤 oracle이 실행하도록 구성된 작업 목록입니다.

| 이름        | 응답 유형     | ID                                 | Adapters                                                                                      |
| --------- | --------- | ---------------------------------- | --------------------------------------------------------------------------------------------- |
| HTTP GET  | `uint256` | `da20aae0e4c843f6949e5cb3f7cfe8c4` | `httpget`<br/>`jsonparse`<br/>`multiply`<br/>`ethuint256`<br/>`ethtx` |
| HTTP GET  | `int256`  | `e0c76e45462f4e429ba32c114bfbf5ac` | `httpget`<br/>`jsonparse`<br/>`multiply`<br/>`ethint256`<br/>`ethtx`  |
| HTTP GET  | `bool`    | `999539ec63414233bdc989d8a8ff10aa` | `httpget`<br/>`jsonparse`<br/>`ethbool`<br/>`ethtx`                         |
| HTTP GET  | `bytes32` | `a82495a8fd5b4cb492b17dc0cc31a4fe` | `httpget`<br/>`jsonparse`<br/>`ethbytes32`<br/>`ethtx`                      |
| HTTP POST | `bytes32` | `a82495a8fd5b4cb492b17dc0cc31a4fe` | `httppost`<br/>`jsonparse`<br/>`ethbytes32`<br/>`ethtx`                     |

작업 사양에 대한 자세한 내용은 [여기](https://docs.chain.link/docs/job-specifications)를 참조하십시오.

전체 Chainlink API 참조는 [여기](https://docs.chain.link/docs/chainlink-framework)에서 찾을 수 있습니다.
