---
id: index
title: PlasmaClient
keywords:
  - 'maticjs, plasma client, contract, polygon, sdk'
description: 'PlasmaClient를 사용하면 POS Bridge와 상호 작용할 수 있습니다.'
---

# 플라즈마 브리지

플라즈마 브리지 기능은 [별도의 저장소](https://github.com/maticnetwork/maticjs-plasma)에서 사용할 수 있습니다. 따라서 `플라즈마` 브리지를 사용하기 위해서는 별도의 패키지를 설치해야 합니다.

## 설치

```
npm i @maticnetwork/maticjs-plasma
```

## 설정

`PlasmaClient`는 **Plasma** Bridge와 상호 작용하는 데 사용할 수 있습니다.

```
import { PlasmaClient } from "@maticnetwork/maticjs-plasma"

const plasmaClient = new PlasmaClient();

await plasmaClient.init({
    network: <network name>,  // 'testnet' or 'mainnet'
    version: <network version>, // 'mumbai' or 'v1'
    parent: {
      provider: <parent provider>,
      defaultConfig: {
            from: <from address>
      }
    },
    child: {
      provider: <child provider>,
      defaultConfig: {
            from: <from address>
      }
    }
});

```

`plasmaClient`가 시작되면 사용 가능한 모든 API와 상호 작용할 수 있습니다.
