---
id: did-implementation
title: 폴리곤 DID 구현
sidebar_label: Identity
description: 폴리곤의 DID 구현에 대해 알아보기
keywords:
  - docs
  - 폴리곤
  - matic
  - DID
  - identity
image: https://matic.network/banners/matic-network-16x9.png
slug: did-implementation/getting-started
---

폴리곤 팀에서 게시한 구현 패키지를 사용하여 폴리곤 DID를 생성하고 폴리곤 원장에 게시하려는 사용자를 위한 시작 안내서입니다.

폴리곤 DID 방식 구현은 3개의 패키지로 구성됩니다. 즉, polygon-did-registrar, polygon-did-resolver 및 polygon-did-registry-contract입니다. 폴리곤 네트워크에서 DID를 등록하거나 읽는 기능을 통합하려는 사용자는 다음 가이드를 사용할 수 있습니다.

DID는 본질적으로 중앙 기관 없이 생성된 고유 식별자입니다.  검증 가능한 자격 증명의 맥락에서 DID는 문서에 서명하는 데 사용되므로 필요할 때 사용자가 문서의 소유권을 쉽게 증명할 수 있습니다.

## 폴리곤 DID 메소드

폴리곤 DID 메소드 정의는 DID-Core 사양 및 표준을 따릅니다. DID URI는 콜론으로 구분된 세 가지 구성 요소, 스키마, 메소드 이름, 마지막으로 메소드 특정 식별자로 구성됩니다. 폴리곤의 경우 URI는 다음과 같습니다.
```
did:polygon:<Ethereum address>
```
여기서 스키마는 'did', 메소드 이름은 'polygon', 메소드별 식별자는 이더리움 주소입니다.

## 폴리곤 DID 구현

폴리곤 DID는 두 개의 패키지를 사용하여 구현할 수 있습니다. 사용자는 각각의 npm 라이브러리를 가져와서 각각의 애플리케이션에 폴리곤 DID 방법론을 통합하는 데 사용할 수 있습니다. 구현에 대한 세부 정보는 다음 섹션에서 제공됩니다.

## DID 생성

시작하려면 먼저 DID를 만들어야 합니다. 폴리곤의 경우 생성은 사용자가 스스로 DID uri를 생성해야 하고 다음으로 폴리곤 원장에 등록해야 하는 두 단계의 캡슐화입니다.

### Step 1 - DID 생성하기

프로젝트에서 폴리곤 DID URI를 생성하려면 먼저 설치해야 합니다.
```
npm i @ayanworks/polygon-did-registrar --save
```
설치가 완료되면 사용자는 다음과 같이 사용할 수 있습니다.
```
import { createDID } from "polygon-did-registrar";
```
createdDID 함수는 사용자가 DID URI를 생성하는 데 도움이 됩니다. DID를 생성하는 동안 두 가지 시나리오가 있을 수 있습니다.

사용자가 이미 지갑을 소유하고 있으며 동일한 지갑에 해당하는 DID를 생성하고자 합니다.
```
const {address, publicKey58, privateKey, DID} = await createDID(network, privateKey);
```
2) 사용자가 기존 지갑이 없고 지갑을 생성하고자 하는 경우 다음을 사용할 수 있습니다.
```
const {address, publicKey58, privateKey, DID} = await createDID(network);
```
두 경우 모두 네트워크 매개변수는 사용자가 폴리곤 테스트 네트워크에서 DID를 생성할 것인지 폴리곤 메인 네트워크에서 생성할 것인지를 나타냅니다.

샘플 입력
```
network :"testnet | mainnet"
privateKey? : "0x....."
```
따라서 1단계가 끝나면 DID URI가 생성됩니다.
```
DID mainnet: did:polygon:0x...
DID testnet: did:polygon:testnet:0x...
```

### Step 2 - DID 등록하기

DID URI와 해당 DID 문서를 원장에 등록하려면 먼저 다음과 같이 `polygon-did-registrar`를 사용해야 합니다.
```
import { registerDID } from "polygon-did-registrar";
```
DID 등록의 전제 조건으로 사용자는 DID에 해당하는 지갑에 필요한 토큰 잔고가 있는지 확인해야 합니다. 사용자가 지갑에 토큰 잔고가 있으면 아래와 같이 registerDID 기능을 호출할 수 있습니다.
```
const txHash = await registerDID(did, privateKey, url?, contractAddress?);
```
매개변수 `did`와 `privateKey`는 필수이며 `url`과 `contractAddress`를 입력하는 것은 선택사항입니다. 사용자가 마지막 두 매개변수를 제공하지 않으면 라이브러리는 DID URI에서 네트워크의 기본 구성을 선택합니다.

모든 매개변수가 사양과 일치하고 모든 것이 올바른 순서로 제공되면 registerDID 함수가 트랜잭션 해시를 반환하고 그렇지 않으면 해당 오류가 반환됩니다.

이것으로 폴리곤 네트워크에 DID를 등록하는 작업을 성공적으로 완료했습니다.

## DID 해결하기

시작하려면 다음 라이브러리를 설치하십시오.
```
npm i @ayanworks/polygon-did-resolver --save
```
그리고
```
npm i did-resolver --save
```

원장에 등록된 DID 문서를 읽으려면 DID 폴리곤 URI를 가진 모든 사용자가 먼저 프로젝트에서 가져올 수 있습니다.
```
import * as didResolvers from "did-resolver";
import * as didPolygon from '@ayanworks/polygon-did-resolver';
```
패키지를 가져온 후 다음을 사용하여 DID 문서를 검색할 수 있습니다.
```
const myResolver = didPolygon.getResolver()
const resolver = new DIDResolver(myResolver)

const didResolutionResult = this.resolver.resolve(did)
```
여기서 didResolutionResult 객체는 다음과 같습니다.
```
didResolutionResult:
{
    didDocument,
    didDocumentMetadata,
    didResolutionMetadata
}
```

사용자가 DID를 해결하려고 시도하는 동안 가스 비용이 수반되지 않는다는 점에 유의해야 합니다.

## DID 문서 업데이트하기

DID 문서를 업데이트하는 기능으로 프로젝트를 캡슐화하려면 사용자는 먼저 다음과 같이 `polygon-did-registrar`를 사용해야 합니다.
```
import { updateDidDoc } from "polygon-did-registrar";
```
다음은 단지 함수를 호출하는 것입니다.
```
const txHash = await updateDidDoc(did, didDoc, privateKey, url?, contractAddress?);
```
DID 문서를 업데이트하려면 DID 소유자만 요청을 보낼 수 있습니다.

여기에 있는 프라이빗 키에는 해당하는 Matic 토큰도 있어야 합니다.

사용자가 `url` 및 `contractAddress`로 구성을 제공하지 않으면, 라이브러리는 DID URI에서 네트워크의 기본 구성을 선택합니다.

## DID 문서 삭제하기

폴리곤 DID 구현을 통해 사용자는 원장에서 자신의 DID 문서를 취소할 수도 있습니다. 사용자는 먼저 다음과 같이 `polygon-did-registrar`를 사용해야 합니다.
```
import { deleteDidDoc } from "polygon-did-registrar";
```
그리고 나서 다음을 사용합니다.
```
const txHash = await deleteDidDoc(did, privateKey, url?, contractAddress?);
```

매개변수 중 `url` 및 `contractAddress`는 선택적 매개변수로, 사용자가 제공하지 않으면 DID URI를 기반으로 함수가 기본 구성을 선택합니다.

DID의 네트워크 구성에 따라 개인 키가 필요한 Matic 토큰을 보유하는 것이 중요합니다. 그렇지 않으면 트랜잭션이 실패합니다.


## 리포지토리에 기여하기

표준 포크, 분기 및 풀 요청 워크플로를 사용하여 리포지토리에 대한 변경 사항을 제안합니다. 예를 들어 문제 또는 버그 번호를 포함하여 분기 이름을 유익한 정보로 만드십시오.

### 깃허브 리포지토리

```
https://github.com/ayanworks/polygon-did-registrar
```

```
https://github.com/ayanworks/polygon-did-resolver
```