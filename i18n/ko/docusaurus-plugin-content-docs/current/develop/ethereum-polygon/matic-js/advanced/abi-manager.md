---
id: abi-manager
title: ABIManager
keywords:
  - 'abi manager, api type, read, write, polygon'
description: 'matic.js는 내부적으로 ABIManager를 사용하여 ABI 관리를 처리합니다.'
---

`matic.js`는 ABI 관리, 구성을 처리하기 위해 내부적으로 `ABIManager`를 사용합니다. 모든 ABI 및 구성은 [정적 저장소](https://github.com/maticnetwork/static)에서 가져옵니다.

## ABI 변경하기

특히 컨트랙트를 개발할 때 ABI를 변경해야 하는 경우가 있습니다. `ABIManager`를 사용하여 그렇게 할 수 있습니다.

**구문**

```
import { ABIManager } from '@maticnetwork/maticjs'


const manager = new ABIManager(<network name>,<version>);
await manager.init();

// set abi

manager.setABI(<contract name>,<bridge type>, <abi value>);

// get abi

manager.getABI(<contract name>,<bridge type>);
```

네트워크 이름, 컨트랙트 이름, 브리지 이름 등은 [공식 정적 저장소](https://github.com/maticnetwork/static/tree/master/network)에서 가져올 수 있습니다.

**예시**

```
import { ABIManager } from '@maticnetwork/maticjs'


const manager = new ABIManager('testnet','mumbai');
await manager.init();

// set abi

manager.setABI('ERC20PredicateProxy','pos', 'abi value');

// get abi

manager.getABI('ERC20PredicateProxy','pos');
```




