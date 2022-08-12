---
id: set-proof-api
title: ProofApi 설정하기
keywords:
  - setProofApi
  - 폴리곤
  - sdk
description: proof api 구성
---

**더 빠른** 후치열이 있는 일부 API는 프로세스를 더 빠르게 합니다.  이는 누구나 호스팅할 수 있는 백엔드에서 증명 생성 API를 사용하여 수행합니다.

폴리곤은 누구나 사용할 수 있는 증명 생성 API를 호스팅했습니다. API URL은 -  [https://apis.matic.network/](https://apis.matic.network/) 입니다.

`setProofApi` 는 proof api url을 설정하는 데 사용할 수 있습니다.

```
import { setProofApi } from '@maticnetwork/maticjs'

setProofApi("https://apis.matic.network/");
```

👉더 나은 성능을 제공할 proof API를 직접 호스팅하는 것이 좋습니다. 폴리곤에서 제공하는 기본 API는 여러 사람이 사용하기 때문에 성능 문제가 있을 수 있습니다.

여기에 proof api 저장소 링크가 있습니다 - [https://github.com/maticnetwork/proof-generation-api](https://github.com/maticnetwork/proof-generation-api)

api를 배포한 후`setProofApi`를 사용하여 matic.js에서 api url을 설정할 수 있습니다.

예 - proof api를 배포했고 기본 url이 `https://abc.com/`이면, `setProofApi`에서 기본 url을 설정해야 합니다.

```
import { setProofApi } from '@maticnetwork/maticjs'

setProofApi("https://abc.com/");
```


일부 API는 특히 증명이 생성되는 곳에서 많은 RPC 호출을 수행하고 공개 RPC에서는 매우 느릴 수 있기 때문에 더 빠른 API를 사용하는 것이 좋습니다. >
