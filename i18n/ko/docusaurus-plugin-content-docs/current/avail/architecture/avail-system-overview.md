---
id: avail-system-overview
title: 시스템 개요
sidebar_label: 시스템 개요
description: Avail 체인의 아키텍처에 대해 알아보세요.
keywords:
  - docs
  - 폴리곤
  - avail
  - data
  - 유효성
  - 아키텍쳐
image: https://matic.network/banners/matic-network-16x9.png
slug: avail-system-overview
---

<!-- Page is WIP -->

Avail은 Kate 다항식 약정, 삭제 코딩 및 다른 기술들을 활용하여 전체 블록을 다운로드 없이 (체인의 _headers_만 다운로드하는)light clients들이 전체 가용성을 검증할 소량의 블록체인터를 효율적이고 무작위로 샘플링할 수 있게 합니다.

:::정보 Avail에는 실행 환경이 없습니다.

Avail은 스마트 계약 자체를 실행하지 않지만 다른 체인이 Avail을 통해 트랜잭션 데이터를 사용할 수 있도록 합니다. 이러한 체인은 EVM, Wasm 또는 기타 모든 종류의 자체 실행 환경을 구현할 수 있습니다.

:::

:::정보  Avail은 데이터가 무엇인지 상관하지 않습니다.

Avail은 블록 데이터를 사용할 수 있음을 보장하지만 해당 데이터가 무엇인지는 신경 쓰지 않습니다. 데이터는 물론 트랜잭션이 될 수 있지만 다른 형태도 취할 수 있습니다.

:::

Avail 네트워크는 유효성 검사기, 풀 노드 및 라이트 클라이언트와 같은 유형의 네트워크 유지 관리자로 구성됩니다.

:::정보  Avail의 목표는 데이터 가용성을 유지하기 위해 풀 노드에 의존하지 않는 것입니다.

  목표는 라이트 클라이언트에 풀 노드와 동일한 DA 보장을 제공하는 것입니다. 사용자는 Avail 라이트 클라이언트를 사용하는 것이 좋습니다. 그러나 사용자는 Avail 풀 노드를 실행할 수 있으며 잘 지원됩니다.

:::

* **검증 노드**
  - 합의에 참여하는 프로토콜 인센티브 풀 노드.

* **Avail (DA) 풀 노드**
  - Avail을 사용하는 모든 애플리케이션에 대해 모든 블록 데이터를 다운로드하여 사용 가능하게 만듭니다.

* **Avail (DA) 라이트 클라이언트:**
  - 헤더를 다운로드하지만 전체 블록은 아닙니다.
  - 블록의 작은 부분을 무작위로 샘플링하여 가용성 확인
  - Avail 네트워크와 상호 작용하기 위해 로컬 API를 노출합니다.

:::주의 로컬 API는 WIP이며 아직 안정적이지 않습니다:::

이를 통해 Avail을 사용하려는 애플리케이션에서 DA 라이트 클라이언트를 포함할 수 있습니다. 그런 다음 다음을 구축할 수 있습니다:

* **App 풀 노드**
  - Avail(DA) 라이트 클라이언트를 포함
  - 특정 appID에 대한 모든 데이터를 다운로드
  - 트랜잭션 실행을 위한 실행 환경 구현
  - 애플리케이션 상태 유지

* **App 라이트 클라이언트**
  - Avail(DA) 라이트 클라이언트를 포함
  - 최종 사용자 대면 기능 구현

Avail 에코시스템은 특정 사용 사례를 지원하는 브리지도 제공합니다. 현재 설계 중인 브리지 중 하나는 Avail에서 사용할 수 있는 데이터의 증명을 이더리움에 게시하여 유효성 검사기를 구축할 수 있는 _attestation bridge_입니다.

Avail 시스템에 대한 자세한 개요는 이 [Polygon 블로그 게시물에 의한 Avail 소개](https://medium.com/the-polygon-blog/introducing-avail-by-polygon-a-robust-general-purpose-scalable-data-availability-layer-98bc9814c048)를 참고하십시오.
