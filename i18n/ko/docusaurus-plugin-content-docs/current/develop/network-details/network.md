---
id: network-rpc-endpoints
title: 네트워크 엔드포인트
sidebar_label: Endpoints
description: 폴리곤 PoS 메인넷 및 테스트넷용 네트워크 엔드포인트
keywords:
  - docs
  - 폴리곤
  - matic
  - 원격 프로시저 호출
  - endpoints
  - rpcs
  - http
  - 웹소켓
  - wss
image: https://matic.network/banners/matic-network-16x9.png
slug: network
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

이 색인 가이드에는뭄바이 테스트넷 및 폴리곤 PoS 메인넷에 대한 네트워크 세부 정보가 포함되어 있으며 관련 RPC 및 노드 엔드포인트가 나열되어 있습니다.

<Tabs
  defaultValue="mainnet"
  values={[
    { label: 'PoS Mainnet', value: 'mainnet', },
 { label: 'PoS Testnet', value: 'mumbai', },
 ]
}>
<TabItem value="mumbai">

## 뭄바이 PoS 테스트넷

뭄바이 테스트넷은 폴리곤 메인넷을 복제하여 테스트에 사용됩니다. 사용자는 [faucet](https://faucet.polygon.technology/에서 테스트넷 토큰을 얻을 수 있습니다.
테스트넷 토큰은 가치가 없으며 MATIC과 같이 가치가 있는 자산과는 다릅니다.
이를 통해 개발자 또는 네트워크 유지 관리자는 구성을 테스트하고 구현을 실험할 수 있습니다.

| 속성                              | 네트워크 세부사항                                                 |
| ---------------------------------- | ---------------------------------------------------------------- |
| NetworkName                        | **Mumbai**                                                       |
| ParentChain                        | **[Goërli](https://goerli.net/)**                                |
| chainId                            | `80001`                                                          |
| Gas Token                          | [MATIC](gas-token)                                               |
| Gas Station                        | [Mumbai Gas Station](https://gasstation-mumbai.matic.today/v2) (learn more [here](https://docs.polygon.technology/docs/develop/tools/polygon-gas-station/))                                      |
| RPC Endpoint                       | [https://rpc-mumbai.matic.today](https://rpc-mumbai.matic.today)         |
| Node Endpoint                      | [wss://rpc-mumbai.matic.today](wss://rpc-mumbai.matic.today)             |
| Heimdall API                       | [https://heimdall.api.matic.today](https://heimdall.api.matic.today)     |
| Block Explorer                     | [https://mumbai.polygonscan.com/](https://mumbai.polygonscan.com/)       |

:::참고  더 자세한 내용은 

네트워크 세부정보를 포함한 다음 [**JSON data**](https://static.matic.network/network/testnet/mumbai/index.json) 를 참고하십시오.

:::

</TabItem>
<TabItem value="mainnet">

## 폴리곤 PoS 메인넷

폴리곤PoS의 기본 토큰은 MATIC이며 가스에 사용됩니다.

| 속성                             | 네트워크 세부사항                                                  |
| ---------------------------------- | ---------------------------------------------------------------- |
| NetworkName                        | **Polygon**                                                      |
| ParentChain                        | **Ethereum**                                                     |
| chainId                            | `137`                                                            |
| Gas Token                          | [MATIC](gas-token)                                               |
| Gas Station                        | [PolygonScan Gas Tracker (**recommended**)](https://polygonscan.com/gastracker) or [Matic Network Gas Station](https://gasstation-mainnet.matic.network/v2) (learn more [here](https://docs.polygon.technology/docs/develop/tools/polygon-gas-station/))                                                                       |
| RPC Endpoint                       | [https://polygon-rpc.com/](https://polygon-rpc.com/)                     | 
| Node Endpoint                      | [wss://rpc-mainnet.matic.network](wss://rpc-mainnet.matic.network)       |
| Heimdall API                       | [https://heimdall.api.matic.network](https://heimdall.api.matic.network) |
| Block Explorer                     | [https://polygonscan.com/](https://polygonscan.com/)       |

:::노트  자세한 내용은 

네트워크 세부정보가 포함된 다음 [**JSON data**](https://github.com/maticnetwork/static/blob/master/network/mainnet/v1/index.json) 
을 참고하십시오.

:::

</TabItem>
</Tabs>

## RPC API 메소드

개발자는 네트워크 엔드포인트를 활용하여 온체인 데이터와 상호작용하고 다양한 유형의 트랜잭션을 네트워크로 보낼 수 있습니다. API는 JSON-RPC 표준을 따릅니다; JSON-RPC는 블록체인 네트워크와 상호작용할 때 일반적으로 사용되는 상태 비보존형, 경량, 원격 프로시저 호출(RPC)프로토콜입니다.

:::info RPC 호출 시작하기

표준 [**폴리곤 JSON-RPC 호출**](https://edge-docs.polygon.technology/docs/get-started/json-rpc-commands/)에 대한 전체 API문서 세트를 방문하여 시작하십시오.

설정이 필요하지 않은 API요청을 시작하거나 실패한 요청을 수정하거나 폴리곤 네트워크에서 새로운 방법을 탐색하려면 [**Composer App**](https://composer.alchemyapi.io?composer_state=%7B%22chain%22%3A2%2C%22network%22%3A401%2C%22methodName%22%3A%22eth_getBlockByNumber%22%2C%22paramValues%22%3A%5B%22latest%22%2Cfalse%5D%7D)을 사용해 보십시오.

:::

사용자는 Polygon PoS 체인과 상호 작용할 때 자신의 노드를 실행하거나 인프라 및 API 서비스 공급자가 제공하는 공용 엔드포인트 중 하나를 사용하여 네트워크에 연결할 수도 있습니다. Dagger는 dApp 및 백엔드 시스템이 소켓 또는 웹 소켓을 통해 실시간으로 블록체인 이벤트를 얻을 수 있는 방법을 제공하므로 체인에서 실시간 업데이트를 얻는 가장 좋은 방법입니다.

### 인프라 제공자

공용 RPC에는 사용량에 따라 트래픽 또는 속도 제한이 있을 수 있습니다. 다음에서 전용 무료 RPC URL에 가입할 수 있습니다:

* [Alchemy](https://www.alchemy.com/)
* [Ankr](https://www.ankr.com/)
* [Blast (Bware Labs)](https://blastapi.io/)
* [BlockPI](https://chains.blockpi.io/#/polygon)
* [Chainstack](https://chainstack.com/build-better-with-polygon/)
* [DataHub (Figment)](https://datahub.figment.io)
* [Getblock](https://getblock.io/en/)
* [Infura](https://infura.io)
* [MaticVigil](https://rpc.maticvigil.com/)
* [Moralis](https://moralis.io)
* [Pocket Network](https://www.portal.pokt.network/)
* [QuickNode](https://www.quicknode.com/chains/matic)
* [SettleMint](https://docs.settlemint.com/docs/polygon-connect-to-a-node)
