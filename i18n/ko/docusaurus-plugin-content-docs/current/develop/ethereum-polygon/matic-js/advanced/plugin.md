---
id: plugin
title: 플러그인
keywords:
  - 'plugin, api type, read, write, polygon'
description: '플러그인을 사용하면 matic.js에 코드를 삽입할 수 있습니다.'
---

플러그인을 사용하여 `matic.js`에 코드를 삽입할 수 있습니다. 패키지를 사용하는 모든 사람에게 제공될 수 있는 일반적인 일반 코드 세트를 작성하는 데 사용할 수 있습니다.

정보 플러그인은 중요한 논리적 부분만 구현하기 때문에 `matic.js`를 가볍게 만듭니다. :::

사실, web3 라이브러리는 우리가 좋아하는 라이브러리를 사용할 수 있게 해주는 플러그인을 사용하여 지원됩니다.

### 플러그인 개발

플러그인은 `IPlugin`을 구현하는 클래스입니다.

```
import { IPlugin } from "@maticnetwork/maticjs";

export class MyPlugin implements IPlugin {

    // variable matic is - default export of matic.js
    setup(matic) {

        // get web3client
        const web3Client = matic.Web3Client ;
    }
}
```

보시다시피 - `matic.js`의 기본 내보내기로 호출될 `setup` 메소드를 구현하기만 하면 됩니다.

### Use 플러그인

`matic.js`는 플러그인을 사용하기 위한 `use` 메소드를 노출합니다.

```
import { use } from '@maticnetwork/maticjs'

use(MyPlugin)
```

여러 플러그인을 사용할 수 있으며 선언된 것과 동일한 순서로 호출됩니다.

**일부 플러그인 저장소는 다음과 같습니다 -**

- [Matic web3.js](https://github.com/maticnetwork/maticjs-web3)
- [Matic ethers](https://github.com/maticnetwork/maticjs-ethers)
- [FxPortal.js](https://github.com/maticnetwork/fx-portal.js)
