---
id: submit-mapping-request
title: 매핑 요청
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---
import useBaseUrl from '@docusaurus/useBaseUrl';

이더리움과 폴리곤 간에 자산을 전송하려면 매핑이 필요합니다. 우리는 동일한 작업을 수행하기 위해 두 개의 브리지를 제공합니다. 브리지에 대한 자세한 내용은 [여기](/docs/develop/ethereum-polygon/getting-started)에서 이해할 수 있습니다.

### 매핑 요청을 제출하는 단계

매핑 요청은 [https://mapper.polygon.technology/](https://mapper.polygon.technology/)에서 제출해야 합니다. 그런 다음 오른쪽 상단 모서리에 있는 "Map New Token" 버튼을 클릭하여 새 매핑 요청을 만들 수 있습니다.

<img src={useBaseUrl("img/token-mapping/mapping-tool.png")} />


- [브리지](/docs/develop/ethereum-polygon/getting-started)의 유형은 **"Choose map type"** 드롭다운에서 선택해야 합니다.
- 토큰 유형은 "ERC20", "ERC721" 및 "ERC1155"로 표시된 세 개의 탭 중에서 전환하여 선택할 수 있습니다. 다른 토큰 표준을 매핑하려면 [Discord](https://discord.com/invite/XvpHAxZ)의 폴리곤 팀에 연락하거나 [여기](https://support.polygon.technology/support/home)에서 티켓을 만들고 티켓 제목에 "Token Mapping"을 유지하세요.
- **"Choose network"**을 통해 매핑을 수행해야 하는 네트워크를 선택할 수 있습니다. 메인넷 매핑의 경우 **이더리움 – 폴리곤 메인넷**을 선택하고 테스트넷 매핑의 경우 **Goerli Testnet - Mumbai**를 선택할 수 있습니다.
- **"Ethereum 토큰 주소"** 필드에 Ethereum/Goerli 토큰 주소를 입력합니다. 토큰 컨트랙트 코드가 [Ethereum](https://etherscan.io/)/[Goerli](https://goerli.etherscan.io/) 블록체인 탐색기에서 검증되었는지 확인하십시오.
- 표준 ERC20/ERC721/ERC1155 하위 토큰이 필요한 경우 **"Polygon Token Address"** 필드를 비워 둘 수 있습니다. 그러나 사용자 지정 하위 토큰( 표준 ERC 기능 + 사용자 지정 기능 )이 필요한 경우 이 [가이드](/docs/develop/ethereum-polygon/pos/mapping-assets)에 따라 사용자 지정 하위 토큰을 만들 수 있습니다. 사용자 지정 하위 토큰을 배포한 후에는 **"Polygon Token Address"** 필드에 컨트랙트 주소를 언급할 수 있습니다. [Polygon](https://polygonscan.com/)/[Mumbai](https://mumbai.polygonscan.com/) 탐색기에서도 하위 토큰 컨트랙트 코드를 확인하십시오.
- 루트 토큰이 확인되면 **이름**, **기호** 및 **십진수** 필드가 자동으로 채워지며 이러한 필드는 편집할 수 없습니다.
- 드롭다운에서 **"Polygon Mintable"** 또는 **"Non Polygon Mintable"** 토큰을 선택할 수 있습니다. Polygon Mintable 토큰에 대한 자세한 내용은 [여기](/docs/develop/ethereum-polygon/mintable-assets)에서 확인할 수 있습니다.
- 커뮤니케이션을 위해 이메일을 언급하는 것은 필수입니다.

사용자 지정 하위 매핑의 경우 매핑 요청을 제출하기 전에 완료해야 하는 체크리스트가 있습니다. 이더리움에 이미 존재하고 폴리곤 체인으로 이동해야 하는 토큰은 "Non Polygon-Mintable" 토큰이라고 할 수 있고, 폴리곤에서 먼저 발행된 다음 이더리움으로 이동할 토큰을 "Polygon Mintable" 토큰이라고 할 수 있습니다. 이 두 가지 유형에 대한 체크리스트를 살펴보겠습니다.

### 매핑 체크리스트

**Non Polygon-Mintable**

1. 입출금 함수는 하위 토큰 컨트랙트에 있습니다. (참조 템플릿 컨트랙트 -  [ERC20](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildERC20.sol#L1492-#L1508), [ERC721](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildERC721.sol#L2157-#L2238), [ERC1155](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildERC1155.sol#L1784-#L1818))
2. ChildChainManagerProxy 주소만 입금 함수를 호출할 수 있는 권한이 있습니다. (ChildChainManagerProxy - on [Mumbai](https://mumbai.polygonscan.com/address/0xb5505a6d998549090530911180f38aC5130101c6/transactions) , on [Polygon Mainnet](https://polygonscan.com/address/0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa/) )
3. Mint 함수는 내부 함수입니다 ( 이것은 내부적으로 입금 함수에 의해 호출됩니다)

**Polygon Mintable ( 가이드 -** [https://docs.polygon.technology/docs/develop/ethereum-polygon/mintable-assets](https://docs.polygon.technology/docs/develop/ethereum-polygon/mintable-assets) )

1. 입출금 함수는 하위 토큰 컨트랙트에 있습니다. (참조 템플릿 컨트랙트 - [ERC20](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC20.sol#L1492-#L1519), [ERC721](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC721.sol#L2160-#L2275), [ERC1155](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC1155.sol#L1784-#L1851))
2. ChildChainManagerProxy 주소만 입금 함수를 호출할 수 있는 권한이 있습니다. (ChildChainManagerProxy - on [Mumbai](https://mumbai.polygonscan.com/address/0xb5505a6d998549090530911180f38aC5130101c6/transactions) , on [Polygon Mainnet](https://polygonscan.com/address/0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa/) )
3. 루트체인 컨트랙트는 표준 [ERC20](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC20.sol#L1481)/[ERC721](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC721.sol#L2169)/[ERC1155](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC1155.sol#L1785)입니다.
4.  루트 컨트랙트의 민트 함수는 해당 토큰인 PredicateProxyAddress에서만 호출할 수 있습니다(각 토큰 유형에 대한 PredicateProxy 주소는 [여기](/docs/develop/ethereum-polygon/mintable-assets#contract-to-be-deployed-on-ethereum)에서 찾을 수 있습니다.)
