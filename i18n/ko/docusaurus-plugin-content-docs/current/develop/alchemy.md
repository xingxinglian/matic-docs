---
id: alchemy
title: Alchemy 이용하기
sidebar_label: Alchemy 이용하기
description: 폴리곤에서 다음 블록체인 앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

# 🌎 Hello World Smart Contract on Polygon

_이 가이드를 완료하는데 예상되는 시간: ~15분_

블록체인 개발이 처음이고 어디서부터 시작해야 할지 모르거나 스마트 컨트랙트를 배포하고 상호 작용하는 방법을 이해하려는 경우 이 가이드가 적합합니다. 가상지갑([메타마스크](https://metamask.io)), [Solidity](https://docs.soliditylang.org/en/v0.8.0/), [Hardhat](https://hardhat.org),및 [Alchemy](https://alchemy.com/?a=polygon-docs) (아직 이것이 무엇을 의미하는지 이해하지 못하더라도 걱정하지 마십시오. 설명하겠습니다!)를 사용하여 폴리곤 뭄바이 테스트 네트워크에서 간단한 스마트 컨트랙트를 만들고 배포하는 과정을 살펴보겠습니다.

질문이 있으시면 언제든지[ Alchemy Discord](https://discord.gg/gWuC7zB)에 문의하십시오!

## Hardhat을 이용하여 스마트 컨트랙트를 생성하고 배포하기

### Step 1: 폴리곤 네트워크에 연결하기

폴리곤 체인에 요청하는 방법에는 여러 가지가 있습니다. 단순화를 위해 자체 노드를 실행할 필요 없이 폴리곤 체인과 통신할 수 있는 블록체인 개발자 플랫폼 및 API인 Alchemy의 무료 계정을 사용할 것입니다. 플랫폼에는 모니터링 및 분석을 위한 개발자 도구도 있습니다. 이 도구는 스마트 컨트랙트 배포의 내부에서 무슨 일이 일어나고 있는지 이해하기 위해 이 튜토리얼에서 활용할 것입니다. 아직 Alchemy 계정이 없다면, [여기에서 무료로 가입할 수 있습니다](https://alchemy.com/?a=polygon-docs).

### Step 2: 앱 생성하기 (및 API키)

Alchemy 계정을 생성한 후에는 앱을 생성하여 API 키를 생성할 수 있습니다. 이를 통해 폴리곤 Mumbai 테스트 네트워크에 요청할 수 있습니다. 테스트넷에 익숙하지 않다면 [이 가이드](https://docs.alchemyapi.io/guides/choosing-a-network)를 확인하십시오.

탐색 바의 "Apps" 위에 마우스를 놓고 "앱 만들기(Create App)"를 클릭하여 Alchemy 대시보드의 "Create App" 페이지로 이동합니다.

앱 이름을 "Hello World"로 지정하고 간단한 설명을 제공하고 환경(앱 장부에 사용됨)에 대해 "Staging"을 선택하고, 체인으로 "Polygon"을 클릭하고 네트워크로 "Polygon Mumbai"를 선택합니다.

"Create app"를 클릭하면 끝입니다! 앱이 아래 표에 나타나야 합니다.

### Step 3: 지갑주소 생성하기

폴리곤은 이더리움을 위한 Layer-2 스케일링 솔루션이기 때문에 이더리움 지갑을 얻고 폴리곤 네트워크에서 트랜잭션을 송수신하기 위해 사용자 지정 폴리곤 URL을 추가해야 합니다. 이 자습서에서는 지갑 주소를 관리하는 데 사용되는 브라우저의 가상 지갑인 메타마스크를 사용합니다. 이더리움의 트랜잭션이 어떻게 작동하는지 더 알고 싶다면 이더리움 재단의 [이 페이지](https://ethereum.org/en/developers/docs/transactions/)를 확인하세요.

Alchemy에서 고객 폴리곤 RPC URL을 가져오려면 Alchemy 대시보드에서 "Hello World" 앱으로 이동하고 오른쪽 상단 모서리에 있는 "View Key"를 클릭합니다. 그런 다음 Alchemy HTTP API 키를 복사하십시오!

[여기](https://metamask.io/download.html)에서 무료로 메타마스크 계정을 다운로드하고 만들 수 있습니다. 계정을 생성했으면 다음 단계에 따라 지갑에 폴리곤 네트워크를 설정하십시오.

1. 메타마스크 지갑의 오른쪽 상단에 있는 드롭다운 메뉴에서 "Settings"을 선택합니다.
2. 왼쪽 메뉴에서 “Networks”를 선택합니다.
3. 다음 매개변수를 사용하여 지갑을 뭄바이 테스트넷에 연결합니다.

    #### 네트워크 이름: 폴리곤 뭄바이 테스트넷

    #### 새로운 RPC URL: https://polygon-mumbai.g.alchemy.com/v2/your-api-key

    #### ChainID: 80001

    #### 심볼: MATIC

    #### 블록 탐색기 URL: https://mumbai.polygonscan.com/

### Step 4: Faucet에서 폴리곤 뭄바이 테스트 MATIC 추가하기

스마트 컨트랙트를 테스트 네트워크에 배포하려면 가짜 MATIC이 필요합니다. MATIC을 얻으려면 [폴리곤 뭄바이 Faucet](https://faucet.polygon.technology/)으로 이동하여 "Mumbai"를 선택하고 "MATIC 토큰"을 선택하고 폴리곤 지갑 주소를 입력한 다음 "Submet”을 클릭합니다. 네트워크 트래픽으로 인해 가짜 ETH를 수신하는 데 시간이 걸릴 수 있습니다. (이 글을 쓰는 시점에는 약 30분 정도 소요되었습니다.) 잠시 후 메타마스크 계정에 ETH가 보일 것입니다!

### Step 5: 잔고 확인하기

잔고가 있는지 재확인하기 위해 [Alchemy의 구성 도구](https://composer.alchemyapi.io/)를 사용하여 [eth\_getBalance](https://docs.alchemy.com/alchemy/apis/polygon-api/eth_getbalance) 요청을 만들어 보겠습니다. "Polygon"을 체인으로, "Polygon Mumbai"를 네트워크로, "eth_getBalance"를 방법으로 선택하고 주소를 입력합니다. 이것은 지갑에 있는 MATIC의 양을 반환할 것입니다. 구성 도구 사용 방법에 대한 지침은 [이 비디오](https://youtu.be/r6sjRxBZJuU)를 확인하십시오!

메타마스크 계정 주소를 입력하고 "Send Request"를 클릭하면 다음과 같은 응답이 표시됩니다:

```
{ "jsonrpc": "2.0", "id": 0, "result": "0xde0b6b3a7640000" }
```

**참고**: 이 결과는 eth가 아닌 wei입니다. Wei는 이더리움의 최소 단위로 사용됩니다. wei에서 eth로의 변환은 1 ETH = 10^18 wei입니다. 따라서 0xde0b6b3a7640000을 10진수로 변환하면 1 ETH와 동일한 1*10^18을 얻게 되며, 이는 액면에 따라 1 MATIC에 매핑될 수 있습니다.

### Step 6: 프로젝트 초기화하기

먼저 프로젝트를 위한 폴더를 만들어야 합니다. [명령줄](https://www.computerhope.com/jargon/c/commandi.htm)로 이동하여 다음을 입력합니다:

```
mkdir hello-world
cd hello-world
```

이제 프로젝트 폴러 안에 있으므로 `npm init` 를 사용하여 프로젝트를 초기화합니다. 아직 npm 이 설치되어 있지 않다면 [이 지침](https://docs.alchemyapi.io/alchemy/guides/alchemy-for-macs#1-install-nodejs-and-npm)을 따르십시오(Node.js도 필요하므로 다운로드하십시오!).

```bash
npm init # (or npm init --yes)
```

설치 질문에 답하는 방법은 중요하지 않습니다. 다음은 참조용으로 수행한 방법입니다:

```
package name: (hello-world)
version: (1.0.0)
description: hello world smart contract
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)

About to write to /Users/.../.../.../hello-world/package.json:

{   
   "name": "hello-world",
   "version": "1.0.0",
   "description": "hello world smart contract",
   "main": "index.js",
   "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
   },
   "author": "",
   "license": "ISC"
}
```

package.json을 승인하고 시작합니다!

### Step 7: [Hardhat](https://hardhat.org/getting-started/#overview) 다운로드하기

Hardhat은 이더리움 소프트웨어를 컴파일, 배포, 테스트 및 디버그하기 위한 개발 환경입니다. 라이브 체인에 배포하기 전에 로컬에서 스마트 컨트랙트 및 dApp을 구축할 때 개발자를 돕습니다.

`hello-world` 프로젝트 실행 내부에:

```
npm install --save-dev hardhat
```

[설치 지침](https://hardhat.org/getting-started/#overview)에 대한 자세한 내용은 이 페이지를 확인하십시오.

### Step 8: Hardhat 프로젝트 만들기

Inside our `hello-world` 프로젝트 폴더내부에, 다음을 실행합니다:

```
npx hardhat
```

그러면 환영 메시지와 원하는 작업을 선택할 수 있는 옵션이 표시됩니다. “create an empty hardhat.config.js”를 선택하십시오:

```
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

👷 Welcome to Hardhat v2.0.11 👷‍

What do you want to do? …
Create a sample project
❯ Create an empty hardhat.config.js
Quit
```

이것은 우리를 위해 `hardhat.config.js` 파일을 생성할 것며, 여기에서 프로젝트에 대한 모든 설정을 특정할 것입니다(step 13에서).

### Step 9: 프로젝트 폴더 추가하기

프로젝트를 체계적으로 유지하기 위해 두 개의 새 폴더를 만듭니다. 명령줄에서 `hello-world` 프로젝트의 루트 디렉터리로 이동하고 다음을 입력합니다:

```
mkdir contracts
mkdir scripts
```

* `contracts/` 는 hello world 스마트 컨트랙트 코드 파일을 보관할 곳입니다.
* `scripts/` 는 배포하고 컨트랙트와 상호 작용할 스크립트를 보관하는 곳입니다.

### Step 10: 컨트랙트 작성하기

여러분은 스스로에게 물어 볼지도 모릅니다. 도대체 언제 코드를 작성하는거지?? 자, 여기 Step 10이 있습니다.😄

여러분이 선호하는 편집기에서 hello-world프로젝트를 엽니다.(우리는 [VSCode](https://code.visualstudio.com)를 선호합니다). 스마트 컨트랙트는 HelloWorld.sol 스마트 컨트랙트를 작성하는데 사용할 Solidity라는 언어로 작성됩니다.

1. "contracts" 폴더로 이동하여 `HelloWorld.sol`이라는 새 파일을 만듭니다.
2. 아래는 이 튜토리얼에서 사용할 [이더리움 재단](https://ethereum.org/en/)의 샘플 Hello World 스마트 컨트랙트입니다. 아래 내용을 복사하여 `HelloWorld.sol file`에 붙여넣고 주석을 읽고 이 컨트랙트가 무엇을 하는지 이해해야 합니다:

```
// SPDX-License-Identifier: None

// Specifies the version of Solidity, using semantic versioning.
// Learn more: https://solidity.readthedocs.io/en/v0.5.10/layout-of-source-files.html#pragma
pragma solidity >=0.8.9;

// Defines a contract named `HelloWorld`.
// A contract is a collection of functions and data (its state). Once deployed, a contract resides at a specific address on the Ethereum blockchain. Learn more: https://solidity.readthedocs.io/en/v0.5.10/structure-of-a-contract.html
contract HelloWorld {

   //Emitted when update function is called
   //Smart contract events are a way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen.
   event UpdatedMessages(string oldStr, string newStr);

   // Declares a state variable `message` of type `string`.
   // State variables are variables whose values are permanently stored in contract storage. The keyword `public` makes variables accessible from outside a contract and creates a function that other contracts or clients can call to access the value.
   string public message;

   // Similar to many class-based object-oriented languages, a constructor is a special function that is only executed upon contract creation.
   // Constructors are used to initialize the contract's data. Learn more:https://solidity.readthedocs.io/en/v0.5.10/contracts.html#constructors
   constructor(string memory initMessage) {

      // Accepts a string argument `initMessage` and sets the value into the contract's `message` storage variable).
      message = initMessage;
   }

   // A public function that accepts a string argument and updates the `message` storage variable.
   function update(string memory newMessage) public {
      string memory oldMsg = message;
      message = newMessage;
      emit UpdatedMessages(oldMsg, newMessage);
   }
}
```

이것은 생성 시 메시지를 저장하고 `update` 함수를 호출하여 업데이트할 수 있는 매우 간단한 스마트 컨트랙트입니다.

### Step 11: 메타마스크와 Alchemy를 프로젝트에 연결하기

Metamask 지갑, Alchemy 계정을 생성하고 스마트 컨트랙트를 작성했습니다. 이제 3개를 연결할 차례입니다.

가상 지갑에서 전송되는 모든 트랜잭션에는 고유한 프라이빗 키를 사용하는 서명이 필요합니다. 프로그램에 이 권한을 제공하기 위해 프라이빗 키(및 Alchemy API 키)를 환경 파일에 안전하게 저장할 수 있습니다.

> 트랜잭션 전송에 대해 자세히 알아보려면 web3를 사용하여 트랜잭션 전송에 대한 [이 자습서](https://docs.alchemyapi.io/alchemy/tutorials/sending-transactions-using-web3-and-alchemy)를 확인하십시오.

먼저 프로젝트 디렉터리에 dotenv 패키지를 설치합니다:

```
npm install dotenv --save
```

그런 다음 프로젝트의 루트 디렉토리에 `.env` 파일을 만들고 여기에 메타마스크 프라이빗 키와 HTTP Alchemy API URL을 추가합니다.

환경 파일의 이름은 `.env`여야 합니다. 그렇지 않으면 환경 파일로 인식되지 않습니다.

이름을 `process.env` 또는 `.env-custom` 등으로 지정하지 마십시오.

경고: git과 같은 버전 제어 시스템을 사용하여 프로젝트를 관리하는 경우 .env 파일을 추적하지 마십시오. .gitignore 파일에 .env를 추가하여 실수로 비밀을 세상에 공개하지 않도록 합니다.

* 프라이빗 키를 내보내려면 [이 지침](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key)들을 따르세요.
* Alchemy HTTP API 키(RPC URL)를 얻으려면 Alchemy 대시보드에서 "Hello World" 앱으로 이동하고 오른쪽 상단 모서리에 있는 "View Key"를 클릭합니다. 그런 다음 Alchemy HTTP API 키를 복사하십시오!

`.env`는 다음과 같아야 합니다:

```
API_URL = "https://polygon-mumbai.g.alchemy.com/v2/your-api-key"
PRIVATE_KEY = "your-metamask-private-key"
```

이를 실제로 코드에 연결하기 위해 Step 13의 `hardhat.config.js` 파일에서 이러한 변수를 참조합니다.

### Step 12: Ethers.js 설치하기

Ethers.js는 [표준 JSON-RPC 메소드](https://docs.alchemyapi.io/alchemy/documentation/alchemy-api-reference/json-rpc)를 보다 사용자 친화적인 방법으로 래핑하여 이더리움과 더 쉽게 상호 작용하고 요청을 할 수 있도록 하는 라이브러리입니다.

Hardhat을 사용하면 추가 도구 및 확장된 기능을 위해 [플러그인](https://hardhat.org/plugins/)들을 매우 쉽게 통합할 수 있습니다. 우리는 컨트랙트 배포를 위해 [Ethers 플러그인](https://hardhat.org/plugins/nomiclabs-hardhat-ethers.html)을 활용할 것입니다([Ethers.js](https://github.com/ethers-io/ethers.js/)에는 아주 깔끔한 컨트랙트 배포 방법이 있습니다).

프로젝트 디렉토리에 다음을 입력합니다:

```bash
npm install --save-dev @nomiclabs/hardhat-ethers "ethers@^5.0.0"
```

다음 단계에서 `hardhat.config.js` 에 ethers도 필요합니다.

### Step 13: hardhat.config.js 업데이트하기

지금까지 여러 종속성과 플러그인을 추가했습니다. 이제 `hardhat.config.js`를 업데이트하여 프로젝트에서 모든 것을 알 수 있도록 해야 합니다.

`hardhat.config.js`를 다음과 같이 업데이트하십시오:

```javascript
/**
* @type import('hardhat/config').HardhatUserConfig
*/

require('dotenv').config();
require("@nomiclabs/hardhat-ethers");

const { API_URL, PRIVATE_KEY } = process.env;

module.exports = {
   solidity: "0.8.9",
   defaultNetwork: "polygon_mumbai",
   networks: {
      hardhat: {},
      polygon_mumbai: {
         url: API_URL,
         accounts: [`0x${PRIVATE_KEY}`]
      }
   },
}
```

### Step 14: 컨트랙트 컴파일하기

지금까지 모든 것이 제대로 작동하는지 확인하기 위해 컨트랙트를 컴파일해 보겠습니다. `compile` 작업은 내장된 hardhat 작업 중 하나입니다.

명령줄에서 다음을 실행합니다:

```bash
npx hardhat compile
```

`SPDX license identifier not provided in source file` 에 대한 경고가 표시될 수 있지만, 걱정할 필요가 없습니다 – 바라건대 다른 모든 것이 좋아 보입니다! 그렇지 않은 경우 항상 [Alchemy discord](https://discord.gg/u72VCg3)에서 메시지를 보낼 수 있습니다.

### Step 15: 배포 스크립트 작성하기

이제 컨트랙트가 작성되고 구성 파일을 사용할 수 있으므로 컨트랙트 배포 스크립트를 작성할 차례입니다.

`scripts/` 폴더로 이동하여 `deploy.js` 라는 새 파일을 만들고 여기에 다음 내용을 추가합니다:

```javascript
async function main() {
   const HelloWorld = await ethers.getContractFactory("HelloWorld");

   // Start deployment, returning a promise that resolves to a contract object
   const hello_world = await HelloWorld.deploy("Hello World!");   
   console.log("Contract deployed to address:", hello_world.address);
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
```

Hardhat은 [Contracts tutorial](https://hardhat.org/tutorial/testing-contracts.html#writing-tests)에서 이러한 각 코드 라인이 수행하는 작업을 설명하는 놀라운 일을 했으며, 여기에서 그들의 설명을 적용했습니다.

```javascript
const HelloWorld = await ethers.getContractFactory("HelloWorld");
```

ethers.js의 `ContractFactory`는 새로운 스마트 컨트랙트를 배포하는 데 사용되는 추상화이므로 여기 `HelloWorld`는 Hello World 컨트랙트의 인스턴스를 위한 [팩토리](https://en.wikipedia.org/wiki/Factory\_\(object-oriented\_programming\))입니다. `hardhat-ethers` 플러그인 `ContractFactory` 및 `Contract`를 사용할 때 인스턴스는 기본적으로 첫 번째 서명자(소유자)에 연결됩니다.

```javascript
const hello_world = await HelloWorld.deploy();
```

`ContractFactory`에서 `deploy()`를 호출하면 배포가 시작되고 `Contract` 개체로 확인되는 `Promise`가 반환됩니다. 이것은 각 스마트 컨트랙트 기능에 대한 메소드가 있는 개체입니다.

### Step 16: 컨트랙트 배포하기

마침내 스마트 컨트랙트를 배포할 준비가 되었습니다! 명령줄로 이동하여 다음을 실행합니다:

```bash
npx hardhat run scripts/deploy.js --network polygon_mumbai
```

그러면 다음과 같은 내용이 표시됩니다:

```bash
Contract deployed to address: 0x3d94af870ED272Cd5370e4135F9B2Bd0e311d65D
```

**이 주소를 복사하여 붙여넣어 어딘가에 저장하십시오**. 이후 자습서에서 이 주소를 사용하므로 잃어버리지 않도록 하십시오.

[Polygon Mumbai explorer](https://mumbai.polygonscan.com/) 로 이동하여 컨트랙트 주소를 검색하면 성공적으로 배포되었음을 확인할 수 있습니다.

`From` 주소는 메타마스크 계정 주소와 일치해야 하며 To 주소는 "Contract Creation"이라고 표시됩니다. 그러나 트랜잭션을 클릭하면 `To` 필드에 컨트랙트 주소가 표시됩니다:

축하합니다! 폴리곤 체인에 스마트 컨트랙트를 배포했습니다 🎉

내부에서 무슨 일이 일어나고 있는지 이해하기 위해 [Alchemy 대시보드](https://dashboard.alchemyapi.io/explorer)의 Explorer 탭으로 이동해 보겠습니다. Alchemy 앱이 여러 개인 경우 앱별로 필터링하고 "Hello World"를 선택해야 합니다.

여기에서 우리가 `.deploy()` 함수를 호출할 때 Hardhat/Ethers가 내부적으로 만든 소수의 JSON-RPC 호출을 볼 수 있습니다. 여기서 호출해야 할 두 가지 중요한 것은 [`eth_sendRawTransaction`](https://docs.alchemyapi.io/alchemy/documentation/alchemy-api-reference/json-rpc#eth\_sendrawtransaction)입니다. 이는 실제로 컨트랙트를 폴리곤 체인에 작성하라는 요청이고, [`eth_getTransactionByHash`](https://docs.alchemyapi.io/alchemy/documentation/alchemy-api-reference/json-rpc#eth\_gettransactionbyhash)는 해시(트랜잭션을 보낼 때의 일반적인 패턴)가 주어진 경우 트랜잭션에 대한 정보를 읽어달라는 요청입니다.

이것이 이 자습서의 전부입니다! 이 자습서를 완료한 후 Twitter [@alchemyplatform](https://twitter.com/AlchemyPlatform)에 태그를 지정하여 귀하의 경험이 어땠는지 또는 피드백이 있는지 알려주십시오!
