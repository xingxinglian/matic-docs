---
id: nftstorage
title: NFTs 발행
description: NFT.스토리지와 폴리곤으로 민팅하기
keywords:
  - nft.storage
  - filecoin
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

이 튜토리얼에서는 NFT.Storage를 통해 폴리곤 블록체인 및 IPFS/Filecoin 저장소를 사용하여 NFT를 발행하는 방법을 알려줍니다. 이더리움용 레이어 2 확장 솔루션인 폴리곤은 이더리움의 EVM과의 완전한 호환성을 유지하면서 속도와 더 낮은 트랜잭션 비용을 위해 개발자가 선택하는 경우가 많습니다. 이 자습서는 표준화된 스마트 컨트랙트의 생성 및 배포, NFT.Storage API를 통해 IPFS 및 Filecoin에 메타데이터 및 자산 저장, 폴리곤의 자체 지갑에 NFT 발행하는 과정을 안내합니다.

## 소개

이 튜토리얼에서 우리는 민팅 프로세스로 세 가지 특성을 충족하는 것을 목표로 할 것입니다.

1. 비용 및 처리량 측면에서 민팅 프로세스의 *확장성(Scalability)*. 사용 사례가 NFT를 신속하게 생성하는 것을 목표로 하는 경우, 기본 기술은 모든 민팅 요청을 처리해야 하며 민팅 비용이 저렴해야 합니다.
2. NFT의 내구성(Durability), 자산의 수명이 길 수 있으므로 전체 수명 동안 계속 사용할 수 있어야 합니다.
3. NFT 및 NFT가 나타내는 자산의 *불변성(Immutability)*. 원치않는 변경과 악의적인 행위자가 NFT가 나타내는 디지털 자산을 변경하는 것을 방지하기 함을 나타냅니다.

[폴리곤](https://polygon.technology)은 프로토콜 및 프레임워크를 통해 *확장성*의 특성을 해결합니다. 또한 이더리움 및 가상 머신과도 호환되므로 개발자가 두 블록체인 간에 코드를 자유롭게 이동할 수 있습니다. 마찬가지로 [NFT.Storage](https://nft.storage)는 기본 [Filecoin](https://filecoin.io) 네트워크의 성능과 IPFS의 [콘텐츠 주소](https://nftschool.dev/concepts/content-addressing/) 지정을 사용하여 *불변성*으로 *내구성*을 보장합니다.

이 튜토리얼에서는 NFT 민팅 프로세스에 대한 개요를 얻고 NFT.Storage로 디지털 자산을 저장하는 방법과 이 디지털 자산을 사용하여 폴리곤에서 NFT를 민팅하는 방법을 배웁니다.

## 전제조건

NFT에 대한 일반적인 지식은 배경과 맥락을 제공합니다. [NFT School은 NFT 기초](https://nftschool.dev/concepts/non-fungible-tokens/), 고급 주제를 다루며 더 많은 자습서를 제공합니다.

이 튜토리얼에서 찾은 코드를 테스트하고 실행하려면 작동하는 [Node.js 설치](https://nodejs.org/en/download/package-manager/)가 필요합니다.

또한 소량의 MATIC 토큰이 있는 뭄바이 테스트넷의 폴리곤 지갑이 필요합니다. 시작하려면 아래 지침을 따르세요:

1. **[메타마스크](https://metamask.io/)를 다운로드하여 설치합니다**. 메타마스크는 암호화폐 지갑이자 블록체인 앱의 관문입니다. 사용하기가 매우 쉽고 폴리곤 지갑 설정과 같은 많은 단계를 단순화합니다.
2. **메타마스크를 폴리곤의 [Mumbai 테스트넷](https://docs.polygon.technology/docs/develop/metamask/overview)**에 연결하고 그것을 드롭다운 메뉴에서 선택합니다. 우리는 폴리곤의 테스트넷을 사용하여 무료로 NFT를 민팅할 것입니다.
3. [faucet](https://faucet.polygon.technology/)를 이용하여 지갑으로 **MATIC 토큰을 받습니다**. 뭄바이 테스트넷을 선택하고 메타마스크의 지갑 주소를 양식에 붙여 넣습니다. NFT를 발행하려면 블록체인에 새로운 트랜잭션을 추가하기 위해 채굴자가 부과하는 수수료인 소량의 MATIC를 지불해야 합니다(예: NFT 발행 또는 새로운 스마트 계약 생성).
4. 오른쪽 상단 모서리에 있는 세 개의 점을 클릭하고 '계정 세부 정보'를 선택하여 메타마스크에서 **프라이빗 키를 복사합니다**. 하단에는 프라이빗 키를 내보내는 버튼이 있습니다. 그것을 클릭하고 메시지가 나타나면 암호를 입력하십시오. 지금은 프라이빗 키를 텍스트 파일에 복사하여 붙여 넣을 수 있습니다. 블록체인과 상호 작용할 때 자습서의 뒷부분에서 사용할 것입니다.

마지막으로 텍스트 또는 코드 편집기가 필요합니다. 더 많은 편의를 위해 JavaScript 및 Solidity 모두에 대한 언어 지원이 있는 편집기를 선택하십시오. 좋은 옵션은 [solidity](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity) 확장이 활성화된  [Visual Studio Code](https://code.visualstudio.com) 입니다.

## 준비

### NFT.storage용 API 키 가져오기

NFT.Storage를 사용하려면 API 키가 필요합니다. 먼저 [NFT.Storage로 이동하여 이메일 주소로 로그인합니다](https://nft.storage/login/). 로그인하는 매직 링크가 포함된 이메일을 받게 됩니다. 비밀번호가 필요하지 않습니다. 성공적으로 로그인한 후 탐색 바를 통해 API 키로 이동합니다. **새로운 키**를 생성하는 버튼을 찾을 수 있습니다. API 키 이름을 묻는 메시지가 표시되면 자유롭게 하나를 선택하거나 "polygon + NFT.Storage"를 사용할 수 있습니다. 지금 키 열의 내용을 복사하거나 자습서의 뒷부분에서 NFT.Storage를 다시 참조할 수 있습니다.

### 작업 공간 설정하기

이 자습서의 작업 공간으로 사용할 수 있는 새로운 빈 폴더를 만듭니다. 파일 시스템의 이름과 위치를 자유롭게 선택하십시오. 터미널을 열고 새로 생성된 폴더로 이동합니다.

다음으로 다음 Node.js 종속물들을 설치합니다:

- **Hardhat 및 Hardhat-Ethers**, 이더리움(및 폴리곤과 같은 이더리움 호환 블록체인)을 위한 개발 환경.
- **OpenZeppelin**, 표준화된 NFT 기본 컨트랙트를 특징으로 하는 스마트 컨트랙트 모음.
- **NFT.Storage**, API에 연결하기 위한 라이브러리.
- **Dotenv**, 구성을 위한 환경 파일을 처리하는 라이브러리 (예: 스크립트에 프라이빗 키 삽입).

다음 명령을 사용하여 모든 종속물들을 한 번에 설치합니다:

```bash
npm install hardhat @openzeppelin/contracts nft.storage dotenv @nomiclabs/hardhat-ethers
```

현재 폴더에서 Hardhat을 초기화해야 합니다. 초기화를 시작하려면 다음을 실행하십시오:

```bash
npx hardhat
```

메시지가 표시되면, `Create an empty hardhat.config.js`를 선택합니다. 콘솔 출력은 다음과 같아야 합니다:

```bash
✔ What do you want to do? · Create an empty hardhat.config.js
✨ Config file created ✨
```

폴리곤 뭄바이 테스트 네트워크를 지원하기 위해 hardhat 구성 파일 `hardhat.config.js`를 약간 수정할 것입니다. 마지막 단계에서 생성한 `hardhat.config.js`를 엽니다. 환경 파일에서 폴리곤 지갑 프라이빗 키를 로드하고 있으며 이 환경 파일을 안전하게 보관해야 합니다. 요구 사항에 따라 다른 rpc [링크](https://docs.polygon.technology/docs/develop/network-details/network)를 사용할 수도 있습니다.

```js
/**
* @type import('hardhat/config').HardhatUserConfig
*/
require("@nomiclabs/hardhat-ethers");
require('dotenv').config();
const { PRIVATE_KEY } = process.env;
module.exports = {
  defaultNetwork: "PolygonMumbai",
  networks: {
    hardhat: {
    },
    PolygonMumbai : {
      url: "https://rpc-mumbai.maticvigil.com",
      accounts: [PRIVATE_KEY]
    }
  },
  solidity: {
    version: "0.8.12",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
}
```

NFT.Storage 및 폴리곤 지갑에 대한 API 키를 보관할 `.env`라는 새 파일을 만듭니다. `.env` 파일의 내용은 아래 목록과 같아야 합니다:

```bash
PRIVATE_KEY="Your Private Key"
NFT_STORAGE_API_KEY="Your Api Key"
```

placeholders를 준비하는 동안 생성한 API 키와 폴리곤 지갑 프라이빗 키로 대체합니다.

프로젝트를 체계적으로 유지하기 위해 세 개의 새 폴더를 만듭니다:

1. `contracts`, Solidity로 작성된 폴리곤 컨트랙트를 위함.
2. `assets`, 우리가 NFT로 민팅할 디지털 자산을 포함합니다.
3. `Scripts`,  준비 및 민팅 프로세스를 주도하는 도우미 역할을 합니다.

다음 명령을 실행합니다:

```bash
mkdir contracts assets scripts
```

마지막으로 `assets` 폴더에 이미지를 추가합니다. 이 이미지는 NFT.Storage에 업로드하고 폴리곤에 민트를 업로드할 작품입니다. 지금은 이름을 `ExampleNFT.png`으로 지정하겠습니다. 멋진 아트가 준비되어 있지 않다면 [간단한 패턴을 다운로드](https://ipfs.io/ipfs/bafkreiawxb4aji744637trok275odl33ioiijsvvahnat2kw5va3at45mu)할 수 있습니다.

## NFT 민팅하기

### NFT.Storage로 자산 데이터 저장하기

NFT.Storage를 사용하여 디지털 자산과 해당 메타데이터를 저장합니다. NFT.Storage는 디지털 자산을 Filecoin 및 IPFS에 자동으로 업로드하여 불변성과 내구성을 보장합니다. IPFS 및 Filecoin은 변경 불가능한 참조를 위해 콘텐츠 식별자(CID)에서 작동합니다. IPFS는 지리적으로 복제된 캐싱으로 빠른 검색을 제공하고 Filecoin은 인센티브를 제공하는 스토리지 공급자를 통해 내구성을 보장합니다.

`scripts` 디렉토리 아래에 `store-asset.mjs`라는 스크립트를 작성하십시오. 내용은 아래와 같습니다:

```js
import { NFTStorage, File } from "nft.storage"
import fs from 'fs'
import dotenv from 'dotenv'
dotenv.config()

const { NFT_STORAGE_API_KEY } = process.env

async function storeAsset() {
   const client = new NFTStorage({ token: NFT_STORAGE_API_KEY })
   const metadata = await client.store({
       name: 'ExampleNFT',
       description: 'My ExampleNFT is an awesome artwork!',
       image: new File(
           [await fs.promises.readFile('assets/MyExampleNFT.png')],
           'MyExampleNFT.png',
           { type: 'image/png' }
       ),
   })
   console.log("Metadata stored on Filecoin and IPFS with URL:", metadata.url)
}

storeAsset()
   .then(() => process.exit(0))
   .catch((error) => {
       console.error(error);
       process.exit(1);
   });
```

스크립트의 주요 부분은 `storeAsset` 함수입니다. 이전에 생성한 API 키를 사용하여 NFT.Storage에 연결하는 새 클라이언트를 생성합니다. 다음으로 이름, 설명 및 이미지로 구성된 메타데이터를 소개합니다. `assets` 디렉토리의 파일 시스템에서 직접 NFT 자산을 읽고 있다는 점에 유의하십시오. 함수가 끝나면 나중에 폴리곤에서 NFT를 만들 때 사용할 메타데이터 URL을 인쇄합니다.

스크립트를 설정한 후 다음을 실행하여 실행할 수 있습니다:

```bash
node scripts/store-asset.mjs
```

출력은 아래 목록과 같아야 합니다. 여기서 `HASH`는 방금 저장한 아트의 CID입니다.

```bash
Metadata stored on Filecoin/IPFS at URL: ipfs://HASH/metadata.json
```

### Polygon에서 NFT 만들기

#### 민팅을 위한 스마트 컨트랙트 작성하기

먼저 NFT를 발행하는 데 사용할 스마트 컨트랙트를 생성합니다. 폴리곤은 이더리움과 호환되므로 [Solidity](https://soliditylang.org)에서 스마트 컨트랙트를 작성합니다. `contracts` 디렉토리 안에 `ExampleNFT.sol`이라는 NFT 스마트 컨트랙트를 위한 새 파일을 만듭니다. 아래 목록의 코드를 복사할 수 있습니다:

```solidity
// Contract based on https://docs.openzeppelin.com/contracts/4.x/erc721
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.12;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract ExampleNFT is ERC721URIStorage, Ownable {
   using Counters for Counters.Counter;
   Counters.Counter private _tokenIds;

   constructor() ERC721("NFT", "ENFT") {}

   function mintNFT(address recipient, string memory tokenURI)
       public onlyOwner
       returns (uint256)
   {
       _tokenIds.increment();

       uint256 newItemId = _tokenIds.current();
       _mint(recipient, newItemId);
       _setTokenURI(newItemId, tokenURI);

       return newItemId;
   }
}
```

유효한 NFT가 되려면 스마트 컨트랙트가 [ERC-721 표준](https://ethereum.org/en/developers/docs/standards/tokens/erc-721/)의 모든 방법을 구현해야 합니다. 우리는 이미 기본 기능 세트를 제공하고 표준을 준수하는 [OpenZeppelin](https://openzeppelin.com) 라이브러리의 구현을 사용합니다.

스마트 컨트랙트의 맨 위에서 세 가지 OpenZeppelin 스마트 컨트랙트 클래스를 가져옵니다:

`\@openzeppelin/contracts/token/ERC721/ERC721.sol`에는 NFT 스마트 컨트랙트가 넘겨 받을 ERC-721 표준의 기본 방법 구현이 포함되어 있습니다. 자산뿐만 아니라 메타데이터도 JSON 파일 오프-체인으로 저장하기 위한 확장인 `ERC721URIStorage`를 사용합니다. 컨트랙트와 마찬가지로 이 JSON 파일은 ERC-721을 준수해야 합니다.

`\@openzeppelin/contracts/utils/Counters.sol`은 1씩만 증가 또는 감소할 수 있는 카운터를 제공합니다. 우리의 스마트 컨트랙트는 카운터를 사용하여 생성된 총 NFT 수를 추적하고 새로운 NFT에 고유 ID를 설정합니다.

`\@openzeppelin/contracts/access/Ownable.sol`은 스마트 컨트랙트에 대한 액세스 제어를 설정하므로 스마트 컨트랙트 소유자(당신)만 NFT를 발행할 수 있습니다.

```
import 구문 뒤에는 counter, constructor 및 실제로 NFT를 민팅하는 메소드가 포함된 맞춤형 NFT 스마트 컨트랙트가 있습니다. 대부분의 힘든 작업은 ERC-721 표준을 준수하는 NFT를 만드는 데 필요한 대부분의 방법을 구현하는 OpenZeppelin에서 넘겨 진 기본 컨트랙트에 의해 수행됩니다.
```

Counter는 생성된 총 NFT 수를 추적하며, 이는 민팅 방법에서 NFT의 고유 식별자로 사용됩니다.

Constructor에서 스마트 컨트랙트의 이름과 기호(지갑에 표시됨)에 대한 두 개의 문자열 인수를 전달합니다. 원하는 대로 변경할 수 있습니다.

마지막으로, 실제로 NFT를 발행할 수 있는 `mintNFT` 메소드가 있습니다. 메소드는 스마트 컨트랙트 소유자만 실행할 수 있도록 `onlyOwner`로 설정됩니다.

`address recipient`는 처음에 NFT를 수신할 주소를 지정합니다.

`string memory tokenURI`는 NFT의 메타데이터를 설명하는 JSON 문서로 해석되어야 하는 URL입니다. 우리의 경우 이미 NFT.Storage에 저장되어 있습니다. 메소드를 실행하는 동안 메타데이터 JSON 파일에 대해 반환된 IPFS 링크를 사용할 수 있습니다.

메소드 내에서 NFT에 대한 새로운 고유 식별자를 수신하도록 카운터를 증가시킵니다. 그런 다음 OpenZeppelin에서 기본 컨트랙트에 의해 제공되는 메소드를 호출하여 새로 생성된 식별자를 사용하여 수신자에게 NFT를 발행하고 메타데이터의 URI를 설정합니다. 이 메소드는 실행 후 고유 식별자를 반환합니다.

#### Polygon에 스마트 컨트랙트 배포하기

이제 스마트 컨트랙트를 폴리곤에 배포할 시간입니다. `scripts` 디렉토리에 `deploy-contract.mjs`라는 새 파일을 만듭니다. 아래 목록의 내용을 해당 파일에 복사하고 저장합니다.

```js
async function deployContract() {
 const ExampleNFT = await ethers.getContractFactory("ExampleNFT")
 const exampleNFT = await ExampleNFT.deploy()
 await exampleNFT.deployed()
 // This solves the bug in Mumbai network where the contract address is not the real one
 const txHash = exampleNFT.deployTransaction.hash
 const txReceipt = await ethers.provider.waitForTransaction(txHash)
 const contractAddress = txReceipt.contractAddress
 console.log("Contract deployed to address:", contractAddress)
}

deployContract()
 .then(() => process.exit(0))
 .catch((error) => {
   console.error(error);
   process.exit(1);
 });
```

컨트랙트 배포는 hardhat 라이브러리에서 제공하는 도우미 함수들로 수행됩니다. 먼저, 제공된 팩토리로 이전 단계에서 생성한 스마트 컨트랙트를 얻습니다. 그런 다음 해당 메소드를 호출하여 배포하고 배포가 완료될 때까지 기다립니다. 테스트넷 환경에서 올바른 주소를 얻기 위해 설명된 코드 아래에 몇 줄 더 있습니다. `mjs` 파일 저장 다음 명령으로 스크립트를 실행합니다:

```bash
npx hardhat run scripts/deploy-contract.mjs --network PolygonMumbai
```

모든 것이 정확하면, 다음 출력이 표시됩니다:

```bash
Contract deployed to address: 0x{YOUR_CONTRACT_ADDRESS}
```

민팅 단계에서 인쇄된 컨트랙트 주소가 필요합니다. 복사하여 별도의 텍스트 파일에 붙여 넣고 나중에 사용할 수 있도록 저장할 수 있습니다. 이는 민팅 스크립트가 해당 특정 컨트랙트의 민팅 방법을 호출할 수 있도록 하기 위해 필요합니다.

#### 폴리곤에 NFT 민팅하기

NFT를 민팅하는 것은 이제 방금 폴리곤에 배포한 컨트랙트를 호출하는 것입니다. `scripts` 디렉토리 안에 `mint-nft.mjs`라는 새 파일을 만들고 아래 목록에서 이 코드를 복사합니다:

```bash
const CONTRACT_ADDRESS = "0x00"
const META_DATA_URL = "ipfs://XX"

async function mintNFT(contractAddress, metaDataURL) {
   const ExampleNFT = await ethers.getContractFactory("ExampleNFT")
   const [owner] = await ethers.getSigners()
   await ExampleNFT.attach(contractAddress).mintNFT(owner.address, metaDataURL)
   console.log("NFT minted to: ", owner.address)
}

mintNFT(CONTRACT_ADDRESS, META_DATA_URL)
   .then(() => process.exit(0))
   .catch((error) => {
       console.error(error);
       process.exit(1);
   });
```

처음 두 줄을 편집하여 이전 배포의 **컨트랙트 주소**와 NFT.Storage로 자산을 저장할 때 반환된 **메타데이터 URL**을 삽입합니다. 나머지 스크립트는 NFT의 소유자가 될 스마트 컨트랙트에 대한 호출과 IPFS에 저장된 메타데이터에 대한 포인터를 설정합니다.

다음으로 스크립트를 실행합니다:

```bash
npx hardhat run scripts/mint-nft.mjs \--network PolygonMumbai
```

당신은 다음 출력을 기대할 수 있습니다:
```bash NFT minted to: 0x{YOUR_WALLET_ADDRESS} ````

이 튜토리얼에서 샘플 코드를 찾고 계십니까? polygon-nft.storage-demo [링크](https://github.com/itsPiyushMaheshwari/Polygon-nft.storage-demo) Github 리포지토리에서 찾을 수 있습니다.

## 결론

이 튜토리얼에서는 폴리곤 및 NFT.Storage를 사용하여 종단 간 NFT를 발행하는 방법을 배웠습니다. 이 기술 조합은 적절한 분산을 가져오고 *확장성*, *내구성* 및 *불변성*을 보장합니다.

우리는 우리의 필요에 맞는 NFT를 민팅하기 위해 맞춤형 스마트 컨트랙트를 배포했습니다. 이 튜토리얼에서는 ERC-721 표준을 기반으로 하는 간단한 예제를 사용했습니다. 그러나 NFT 수명 주기를 제어하는 복잡한 논리를 정의할 수도 있습니다. 더 복잡한 사용 사례의 경우 후속 표준인 [ERC-1155](https://ethereum.org/en/developers/docs/standards/tokens/erc-1155/)를 시작하는 것이 좋습니다. 튜토리얼에서 사용하는 라이브러리인 OpenZeppelin은 NFT 컨트랙트를 생성하는 데 도움이 되는 [컨트랙트 마법사](https://docs.openzeppelin.com/contracts/4.x/wizard)를 제공합니다.

성공적인 민팅은 NFT의 가치 있는 단계의 시작으로 볼 수 있습니다. 그런 다음 NFT를 사용하여 소유권을 증명하고 다른 사용자에게 양도할 수 있습니다. NFT를 양도하는 이유에는 [OpenSea](https://opensea.io)와 같은 NFT 마켓플레이스 중 하나에서의 성공적인 판매 또는 NFT 기반 게임에서 아이템 획득과 같은 다른 유형의 이벤트가 포함될 수 있습니다. NFT의 풍부한 가능성을 탐색하는 것은 확실히 흥미진진한 다음 단계입니다.

NFT.storage로 NFT 프로젝트를 구축하는 데 도움이 필요하면 [Discord](https://discord.gg/Z4H6tdECb9) 및 [Slack](https://filecoinproject.slack.com/archives/C021JJRH26B)의 `#nft-storage` 채널에 가입하는 것이 좋습니다.
