---
id: replit
title: Replit 이용하기
sidebar_label: Replit 이용하기
description: 폴리곤에서 다음 블록체인 앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

[Replit](https://docs.replit.com/tutorials/01-introduction-to-the-repl-it-ide)은 코드를 작성하고 앱을 호스팅할 수 있는 코딩 플랫폼입니다. Replit은 [Solidity 프로그래밍 언어](https://replit.com/@replit/Solidity-starter-beta?v=1)를 지원하므로 Web3 개발자가 스마트 컨트랙트를 만들고 배포할 수 있는 모든 기능을 제공합니다.

이 글에서는[ Replit IDE](https://replit.com/signup) 및 [Replit Solidity development template (Solidity starter beta)](https://replit.com/@replit/Solidity-starter-beta?v=1)을 사용하여 Polygon에서 Solidity 스마트 컨트랙트를 구축하고 배포하는 방법을 설명합니다.

:::참고
Solidity with Replit에 대한 추가 예제는 Replit 시작하기<ins>**(Get started with Replit)(https://blog.replit.com/solidity)**</ins> 문서를 참조하십시오! 또는https://replit.com/@replit/Solidity-starter-beta?v=1 Replit IDE 및 <ins>**Replit Solidity development template (Solidity starter beta)**</ins>을 확인하십시오.
:::

## 전제조건

Replit을 사용하여 폴리곤에 솔리디티 스마트 컨트랙트를 배포하기 위해 로컬 환경이 필요하지 않습니다.

Polygon Mumbai Testnet 및 배포된 컨트랙트와 상호 작용하려면 브라우저 기반 web3 지갑이 필요합니다. 이미 메타마스크를 사용하고 있다면 Replit으로 테스트할 새 계정을 만드는 것이 좋습니다. 메타마스크 인터페이스의 오른쪽 상단 모서리에 있는 계정 아바타를 클릭하면 나타나는 계정 메뉴에서 이 작업을 수행할 수 있습니다.

Solidity 스마트 컨트랙트를 폴리곤에 배포하려면 다음 전제 조건을 모두 설정해야 합니다:

1. [Replit 계정 만들기](https://replit.com/signup)
2. [메타마스크 지갑 다운로드](https://docs.polygon.technology/docs/develop/metamask/hello/)
3. [메타마스크에 폴리곤 구성하기](https://docs.polygon.technology/docs/develop/metamask/config-polygon-on-metamask/)
4. [테스트넷 토큰 얻기](https://faucet.polygon.technology)

## Repl로 작업하기

생성하는 모든 Repl은 완전한 기능을 갖춘 개발 및 제작 환경입니다. Solidity 스타터 Replit을 만들기 위해 다음 절차를 따르십시오:

1. [로그인](https://replit.com/login) 또는 [계정 만들기](https://replit.com/signup). [Replit 계정](https://docs.replit.com/tutorials/01-introduction-to-the-repl-it-ide)을 생성하면,  화면에 대시보드가 포함되어 거기서 보면서, 프로젝트를 생성하고, 계정을 관리할 수 있습니다.
2. 로그인하면 Solidity 스타터 repl을 만들고, 왼쪽 패널에서 **+ Create Repl**을 선택하거나 화면 오른쪽 상단에서 **+**를 선택합니다.
3. [**Solidity starter(beta)**](https://replit.com/@replit/Solidity-starter-beta?v=1) 템플릿을 선택하고 프로젝트에 제목을 지정합니다.
4. **+ Create Repl**을 클릭하여 프로젝트를 생성합니다.

:::참고
Solidity starter repl은 <ins>**[web3 Ethereum JavaScript API](https://web3js.readthedocs.io/en/v1.5.2/)**</ins>를 사용하여, 구축된 친숙한 웹 인터페이스와 함께 제공되며, 이를 사용하여 컨트랙트를 배포하고 상호 작용할 수 있습니다. Replit에서 관리하고 테스트에 최적화된 이더리움 블록체인의 사용자 버전인 Replit의 테스트넷에 배포합니다.
:::

## Polygon에 배포하기

스마트 컨트랙트를 배포하고 상호 작용할 준비가 되도록 위의 전제 조건 목록을 따랐는지 확인하십시오:

1. **실행**(상단)을 클릭하여 모든 관련 패키지를 설치하고 컨트랙트 배포 UI를 시작합니다.
2. MetaMask 지갑을 웹 인터페이스에 연결하고 [뭄바이 테스트넷](https://docs.polygon.technology/docs/integrate/network/)으로 전환합니다.
3. **Connect wallet**을 클릭하고 계정을 선택한 다음 Connect를 선택하십시오.
4. 드롭다운 목록에서 배포할 컨트랙트를 선택합니다.
5. **Deploy**를 클릭하십시오
6. 지갑에서 트랜잭션을 확인하기 위해 나타나는 메타마스크 팝업을 승인하여 컨트랙트를 배포하십시오.
7. [Polyganscan으로 이동](https://mumbai.polygonscan.com/)하여 계정을 검색하고, 배포된 컨트랙트를 보고, 계정 주소를 복사합니다.

컨트랙트가 배포되면 드롭다운 상자 아래에 확장 가능한 상자로 표시됩니다. 확장하여 사용 가능한 모든 다양한 기능을 살펴보십시오. 이제 제공된 사용자 인터페이스를 사용하거나 인터페이스에 표시된 공유 가능한 URL에서 컨트랙트와 상호 작용할 수 있습니다.

## Replit에 게시하기

Replit을 사용하면 프로젝트를 개인 프로필에 게시할 수 있습니다. 게시 후 프로젝트는 다른 사람들이 탐색, 상호 작용, 복제 및 공동 작업할 수 있도록 스포트라이트 페이지에 표시됩니다.

Replit에 게시하려면 다음 단계를 따릅니다:
1. 화면 상단에서 프로젝트 제목을 선택합니다.
2. 프로젝트 이름과 설명을 완성하고 Publish를 클릭합니다.

## 외부 리소스

* [Solidity 및 Replit으로 이더리움 블록체인에서 에스크로 컨트랙트 구축하기](https://docs.replit.com/tutorials/33-escrow-contract-with-solidity)
