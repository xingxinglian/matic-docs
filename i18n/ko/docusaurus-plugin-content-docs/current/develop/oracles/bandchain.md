---
id: bandchain
title: BandChain
sidebar_label: Bandchain
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

밴드 프로토콜을 사용하면 기존 웹 API에서 데이터를 쿼리하고 블록체인에서 사용할 수 있습니다. 개발자는 oracle 요청 및 결제를 용이하게 하는 코스모스 기반 블록체인인 BandChain을 통해 쿼리를 수행한 후 체인간 통신을 통해 dApp의 데이터를 사용할 수 있습니다. Oracle 데이터 통합은 간단한 3단계로 수행할 수 있습니다:

1. **오라클 스크립트 선택하기**

    Oracle 스크립트는 밴드 체인에서 요청되는 데이터 유형을 고유하게 식별하는 해시입니다. 이러한 스크립트는 [**여기**](https://guanyu-devnet.cosmoscan.io/oracle-scripts)에서 찾을 수 있습니다. 이러한 스크립트는 Oracle 요청을 수행하는 동안 매개변수 중 하나로 사용됩니다.

2. **BandChain에 데이터 요청하기**

 이는 두 가지 방법으로 수행할 수 있습니다.

- BandChain 익스플로러 이용하기

    선택한 오라클 스크립트를 클릭한 다음 실행 탭에서 매개변수를 전달하고 BandChain에서 응답을 받을 수 있습니다. 응답에는 결과와 evm 증명도 포함됩니다. 이 증명은 복사해야 하며 최종 단계에서 사용됩니다. 익스플로러를 사용하여 오라클에 쿼리하기 위한 BandChain 문서가 [**여기**](https://docs.bandchain.org/dapp-developers/requesting-data-from-bandchain/requesting-data-via-explorer)에 있습니다.

    <img src={useBaseUrl("img/bandchain/executeoracle.png")} />

    위에 주어진 것은 난수 값을 얻기 위해 오라클에 요청하는 예입니다. 값 100은 Oracle 요청의 max_range 매개변수에 전달됩니다. 응답으로 해시를 받습니다. 이 해시를 클릭하면 응답의 전체 세부 정보가 표시됩니다.

- BandChain-Devnet JS 라이브러리 이용하기

    밴드체인 Devnet 라이브러리를 사용하여 밴드체인을 직접 쿼리할 수 있습니다. 쿼리를 받으면 응답에서 **evm 증명**을 제공합니다. 이 증명은 BandChain 통합의 마지막 단계에 사용할 수 있습니다. BandChain-Devnet JS 라이브러리를 사용하여 오라클에 쿼리하기 위한 BandChain 문서가 [**여기**](https://docs.bandchain.org/dapp-developers/requesting-data-from-bandchain/requesting-data-via-js-library)에 있습니다. 난수 오라클에 대한 요청 페이로드는 다음과 같습니다. 요청 본문이 application/json 형식으로 전달되었는지 확인하십시오.

3. **스마트 컨트랙트에서 데이터 이용하기**

  마지막 단계는 유효성 검사 컨트랙트를 배포하고 Oracle 요청의 응답을 유효성 검사 컨트랙트 상태 변수에 저장하는 것입니다. 이러한 상태 변수가 설정되면 dapp에서 필요할 때 액세스할 수 있습니다. 또한 이러한 상태 변수는 dApp에서 Oracle 스크립트를 다시 쿼리하여 새 값으로 업데이트할 수 있습니다. 아래는 난수 oracle 스크립트를 사용하여 난수 값을 저장하는 유효성 검사 컨트랙트입니다.

  ```jsx
  pragma solidity 0.5.14;
  pragma experimental ABIEncoderV2;

  import "BandChainLib.sol";
  import "IBridge.sol";

  contract SimplePriceDatabase {
    using BandChainLib for bytes;

    bytes32 public codeHash;
    bytes public params;
    IBridge public bridge;

    uint256 public latestPrice;
    uint256 public lastUpdate;

    constructor(
      bytes32 _codeHash , 
      bytes memory _params, 
      IBridge _bridge
    ) public {
      codeHash = _codeHash;
      params = _params;
      bridge = _bridge;
    }

    function update(bytes memory _reportPrice) public {
      IBridge.VerifyOracleDataResult memory result = bridge.relayAndVerify(_reportPrice);
      uint64[] memory decodedInfo = result.data.toUint64List();

      require(result.codeHash == codeHash, "INVALID_CODEHASH");
      require(keccak256(result.params) == keccak256(params), "INVALID_PARAMS");
      require(uint256(decodedInfo[1]) > lastUpdate, "TIMESTAMP_MUST_BE_OLDER_THAN_THE_LAST_UPDATE");

      latestPrice = uint256(decodedInfo[0]);
      lastUpdate = uint256(decodedInfo[1]);
    }
  }
  ```

  배포할 때 3개의 매개변수를 전달해야 합니다. 첫 번째 매개변수는 Oracle 스크립트 해시인 codeHash입니다. 두 번째 매개변수는 Oracle 스크립트 요청 매개변수 객체입니다. 이것은 바이트 형식으로 전달되어야 합니다.  BandChain은 매개변수 JSON 객체를 바이트 형식으로 변환하기 위한 REST API를 제공합니다. API 세부 정보는 [**여기**](https://docs.bandchain.org/references/encoding-params)에서 찾을 수 있습니다. 이 API에서 받은 응답에 0x를 추가해야 합니다. 세 번째 매개변수는 폴리곤 네트워크에 이미 배포된 Bandchain 컨트랙트의 컨트랙트 주소입니다. 밴드 프로토콜은 Polygon TestnetV3: 0x3ba819b03fb8d34995f68304946eefa6dcff7cbf를 지원합니다.

  주목해야 할 또 다른 사항은 유효성 검사 컨트랙트가 각각 BandChainLib.sol 및 IBridge.sol이라는 도우미 라이브러리와 인터페이스를 가져와야 한다는 것입니다. [**Bandchain**](https://docs.bandchain.org/references/bandchainlib-library) Library and [**IBridge**](https://docs.bandchain.org/references/ibridge-interface) interface 링크에서 찾을 수 있습니다.

  유효성 검사 컨트랙트가 배포되면 dApp에서 쿼리하여 상태 변수에 액세스할 수 있습니다. 유사하게 서로 다른 내장 Oracle 스크립트에 대해 여러 유효성 검사 컨트랙트를 생성할 수 있습니다. IBridge 인터페이스에는 유효성 검사 컨트랙트에서 매번 업데이트되는 값을 확인하는 relayAndVerify라는 메소드가 있습니다. 유효성 검사 계약의 업데이트 메서드에는 상태 변수를 업데이트하는 논리가 있습니다. Oracle 스크립트를 쿼리하여 얻은 evm 증명을 업데이트 메서드에 전달해야 합니다. 값이 업데이트될 때마다 폴리곤에 배포된 BandChain 컨트랙트는 데이터를 계약 상태 변수에 저장하기 전에 데이터를 확인합니다.

  bandChain은 dApp이 스마트 계약 논리를 향상시키는 데 사용할 수 있는 탈중앙화된 오라클 네트워크를 제공합니다. 컨트랙트 배포, 값 저장 및 업데이트에 대한 BandChain 문서는 [**여기**](https://docs.bandchain.org/dapp-developers/requesting-data-from-bandchain/requesting-data-via-js-library)에서 찾을 수 있습니다.