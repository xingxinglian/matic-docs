---
id: hardhat
title: Hardhat 이용하기
description: Matic에서 다음 블록체인앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## **개발 환경 설정하기**

시작하기 전에 몇 가지 기술 요구 사항이 있습니다. 다음을 설치하십시오:

- [Node.js v10+ LTS and npm](https://nodejs.org/en/) (노드와 함께 제공)
- [Git](https://git-scm.com/)

일단 설치되면, hardhat을 설치하기 위해 빈 폴더로 이동하여 npm init를 실행하고 지침에 따라 npm 프로젝트를 생성해야 합니다. 프로젝트가 준비되면 다음을 실행해야 합니다.

```bash
$ npm install --save-dev hardhat
```
Hardhat 프로젝트를 생성하려면 프로젝트 폴더에서 `npx hardhat`을 실행하십시오. 샘플 프로젝트를 만들고 다음 단계를 수행하여 샘플 작업을 시도하고 샘플 컨트랙트를 컴파일, 테스트 및 배포해 보겠습니다.


샘플 프로젝트는 hardhat-waffle 및 hardhat-ethers를 설치하도록 요청합니다. [이 가이드](https://hardhat.org/getting-started/#quick-start)에서 자세히 알아볼 수 있습니다.

## **hardhat-config**

- hardhat.config.js로 이동합니다.
- matic-network-credentials로 hardhat-config 업데이트합니다.
- 프라이빗 키를 저장할 루트에 .env 파일을 만듭니다.
- Polygonscan에서 컨트랙트를 확인하기 위해 .env 파일에 Polygonscan API 키를 추가합니다. [계정 생성하기](https://polygonscan.com/register)로 API 키를 생성할 수 있습니다.

```js
require('dotenv').config();
require("@nomiclabs/hardhat-ethers");
require("@nomiclabs/hardhat-etherscan");

module.exports = {
  defaultNetwork: "matic",
  networks: {
    hardhat: {
    },
    matic: {
      url: "https://rpc-mumbai.maticvigil.com",
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: process.env.POLYGONSCAN_API_KEY
  },
  solidity: {
    version: "0.7.0",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
}
```

> 컨트랙트(들)에 필요한 사항에 따라 여기에서 Solidity 컴파일러 버전을 업데이트해야 합니다.

## **스마트 컨트랙트 파일 컴파일하기**

```bash
$ npx hardhat compile
```

## **Matic 네트워크에 배포하기**

프로젝트 디렉토리의 루트에서 다음 명령을 실행합니다:
```bash
$ npx hardhat run scripts/sample-script.js --network matic
```

컨트랙트 Matic의 Mumbai Testnet에 배포되며 다음과 같습니다:

```shell
Compilation finished successfully
Greeter deployed to: 0xfaFfCAD549BAA6110c5Cc03976d9383AcE90bdBE
```

> 당신의 주소는 다를 수 있음을 기억하십시오. 위는 구조에 대한 아이디어를 제공하기 위한 것입니다. **축하합니다!** Greeter 스마트 컨트랙트를 성공적으로 배포했습니다. 이제 스마트 컨트랙트와 상호 작용할 수 있습니다.

다음에서 배포 상태를 확인할 수 있습니다: https://mumbai.polygonscan.com/

## **Polygonscan에서 컨트랙트 확인하기**

다음 명령어를 실행하여 Polygonscan에서 컨트랙트를 빠르게 확인하세요. 이렇게 하면 배포된 컨트랙트의 소스 코드를 누구나 쉽게 볼 수 있습니다. 복잡한 인수 목록이 있는 constructor가 있는 컨트랙트의 경우 [여기](https://hardhat.org/plugins/nomiclabs-hardhat-etherscan.html)를 참조하세요.

```bash
$ npm install --save-dev @nomiclabs/hardhat-etherscan
$ npx hardhat verify --network matic 0xfaFfCAD549BAA6110c5Cc03976d9383AcE90bdBE
```

> 당신의 주소를 배포된 컨트랙트 주소로 업데이트하는 것을 잊지 마십시오. 명령이 성공하면 Polygonscan에서 확인된 컨트랙트를 볼 수 있습니다!
