---
id: network-agnostics
title: 네트워크 불가지론적 트랜잭션
sidebar_label: 네트워크 불가지론적 트랜잭션
description: Matic에서 다음 블록체인앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## 목표

메타마스크에서 공급자를 변경하지 않고 폴리곤 체인에서 트랜잭션 실행 (이 튜토리얼은 메타마스크의 인페이지 제공자를 대상으로 하며, 다른 제공자의 트랜잭션을 실행하도록 수정할 수 있습니다)

후드 아래에서 사용자는 트랜잭션을 실행하려는 의도에 서명합니다. 트랜잭션은 폴리곤 체인에 배포된 컨트랙트에서 실행하기 위해 간단한 중계자가 이를 중계합니다.

---

- 📺**데모:** [https://www.youtube.com/watch?v=ETvnnZGQDDc&feature=youtu.be](https://www.youtube.com/watch?v=ETvnnZGQDDc&feature=youtu.be)
- 📺**이더온라인 데모**: [https://youtu.be/5tKzMcflOcY?t=1431](https://youtu.be/5tKzMcflOcY?t=1431)
- 👩🏻‍💻**코드**: [https://github.com/nglglhtr/ETHOnline-Workshop/tree/master/2-network-agnostic-transfer](https://github.com/nglglhtr/ETHOnline-Workshop/tree/master/2-network-agnostic-transfer)
- 📄**이더온라인을 위한 도움 문서:** [Build on Matic: [Helper Doc]](https://www.notion.so/Build-on-Matic-Helper-Doc-60650299256f4c1c9e90bae365cbd88e)

## 트랜잭션 실행을 가능하게 하는 것은 무엇인가?

사용자가 상호 작용하는 클라이언트(웹 브라우저, 모바일 앱 등)는 블록체인과 상호 작용하지 않으며 대신 GSN 또는 메타 트랜잭션 솔루션이 작동하는 방식과 유사한 간단한 중계 서버(또는 중계 네트워크)와 상호 작용합니다 (참조:  [메타 트랜잭션: 소개](https://www.notion.so/Meta-Transactions-An-Introduction-8f54cf75321e4ec3b6d755e18e406590)).

블록체인 상호 작용이 필요한 모든 작업에 대해,

- 클라이언트가 사용자에게 EIP712 형식 서명을 요청합니다.
- •	서명은 간단한 릴레이 서버로 전송됩니다(프로덕션에 사용되는 경우 간단한 인증/스팸 보호가 있어야 하거나 biconomy의 mexa sdk를 사용할 수 있음: [https://github.com/bcnmy/mexa-sdk](https://github.com/bcnmy/mexa-sdk))
- 중계자는 블록체인과 상호 작용하여 사용자의 서명을 컨트랙트에 제출합니다.  `executeMetaTransaction`이라는 컨트랙트의 함수는 서명을 처리하고 요청된 트랜잭션을 실행합니다(내부 호출을 통해).
- 중계자가 가스 비용을 지불하여 거래를 효과적으로 무료로 만듭니다🤑

## dApp에 Network Agnostic Transactions 통합하기

- 맞춤형 단순 중계 노드와 biconomy 중에서 선택하십시오.

  - biconomy의 경우, 대시보드에서 dapp을 설정하고 api-id 및 api-key를 저장합니다. 다음을 참조하세요:  [Tutorial: Biconomy](https://www.notion.so/Tutorial-Biconomy-7f578bfb4e7d4904b8c79522085ba568) 또는 [https://docs.biconomy.io/](https://docs.biconomy.io/)

    **단계**

    1. biconomy 대시보드에 컨트랙트를 등록합니다.
       1. [biconomy에 대한 공식문서](https://docs.biconomy.io/biconomy-dashboard)를 방문하십시오.
       2. dapp 등록 시 `Polygon Mumbai` 를 선택합니다.
    2. 프론트엔드에 `API key`를 복사합니다.
    3. 그리고 Manage-Api에  `executeMetaTransaction`  함수를 추가하고 meta-tx를 활성화해야 합니다. ('native-metatx' 옵션 체크)

  - 블록체인에서 서명된 트랜잭션을 보내는 자체 사용자 지정 API를 사용하려면, 다음에서 서버코드를 참조할 수 있습니다: [https://github.com/nglglhtr/ETHOnline-Workshop/tree/master/2-network-agnostic-transfer](https://github.com/nglglhtr/ETHOnline-Workshop/tree/master/2-network-agnostic-transfer)

- 상호 작용하려는 컨트랙트가  `NativeMetaTransactions`에서 상속되는지 확인하세요. 👀 컨트랙트의 `executeMetaTransaction`함수를 살펴보세요.
- Link: [https://github.com/maticnetwork/pos-portal/blob/34be03cfd227c25b49c5791ffba6a4ffc9b76036/flat/ChildERC20.sol#L1338](https://github.com/maticnetwork/pos-portal/blob/34be03cfd227c25b49c5791ffba6a4ffc9b76036/flat/ChildERC20.sol#L1338)

```jsx

let data = await web3.eth.abi.encodeFunctionCall({
    name: 'getNonce', 
    type: 'function', 
    inputs: [{
        name: "user",
        type: "address"
      }]
  }, [accounts[0]]);

  let _nonce = await web3.eth.call ({
    to: token["80001"],
    data
  });

  const dataToSign = getTypedData({
    name: token["name"],
    version: '1',
    salt: '0x0000000000000000000000000000000000000000000000000000000000013881',
    verifyingContract: token["80001"],
    nonce: parseInt(_nonce),
    from: accounts[0],
    functionSignature: functionSig
  });

  const msgParams = [accounts[0], JSON.stringify(dataToSign)];

  let sig = await eth.request ({
    method: 'eth_signTypedData_v3', 
    params: msgParams
  });

  ```


- 중계자와 컨트랙트 설정이 완료되면 클라이언트가 EIP712 형식 서명을 가져와 필요한 매개변수를 사용하여 API를 간단히 호출할 수 있어야 합니다.

    ref: [https://github.com/nglglhtr/ETHOnline-Workshop/blob/6b615b8a4ef00553c17729c721572529303c8e1b/2-network-agnostic-transfer/sign.js#L47](https://github.com/nglglhtr/ETHOnline-Workshop/blob/6b615b8a4ef00553c17729c721572529303c8e1b/2-network-agnostic-transfer/sign.js#L47)

    ```jsx

    let data = await web3.eth.abi.encodeFunctionCall({
        name: 'getNonce',
        type: 'function',
        inputs: [{
            name: "user",
            type: "address"
          }]
      }, [accounts[0]]);

      let _nonce = await web3.eth.call ({
        to: token["80001"],
        data
      });

      const dataToSign = getTypedData({
        name: token["name"],
        version: '1',
        salt: '0x0000000000000000000000000000000000000000000000000000000000013881',
        verifyingContract: token["80001"],
        nonce: parseInt(_nonce),
        from: accounts[0],
        functionSignature: functionSig
      });
      const msgParams = [accounts[0], JSON.stringify(dataToSign)];

      let sig = await eth.request ({
        method: 'eth_signTypedData_v3',
        params: msgParams
      });
    ```

    API 호출하기, ref:[https://github.com/nglglhtr/ETHOnline-Workshop/blob/6b615b8a4ef00553c17729c721572529303c8e1b/2-network-agnostic-transfer/sign.js#L110](https://github.com/nglglhtr/ETHOnline-Workshop/blob/6b615b8a4ef00553c17729c721572529303c8e1b/2-network-agnostic-transfer/sign.js#L110)

    ```jsx
    const response = await request.post(
        'http://localhost:3000/exec', {
          json: txObj,
        },
        (error, res, body) => {
          if (error) {
            console.error(error)
            return
          }
          document.getElementById(el).innerHTML =
          `response:`+ JSON.stringify(body)
        }
      )
    ```

    Biconomy를 사용하는 경우 다음을 호출해야 합니다:

    ```jsx
    const response = await request.post(
        'https://api.biconomy.io/api/v2/meta-tx/native', {
          json: txObj,
        },
        (error, res, body) => {
          if (error) {
            console.error(error)
            return
          }
          document.getElementById(el).innerHTML =
          `response:`+ JSON.stringify(body)
        }
      )
    ```

    여기서 `txObj` 는 다음과 같아야 합니다:

    ```json
    {
        "to": "0x2395d740789d8C27C139C62d1aF786c77c9a1Ef1",
        "apiId": <API ID COPIED FROM THE API PAGE>,
        "params": [
            "0x2173fdd5427c99357ba0dd5e34c964b08079a695",
            "0x2e1a7d4d000000000000000000000000000000000000000000000000000000000000000a",
            "0x42da8b5ac3f1c5c35c3eb38d639a780ec973744f11ff75b81bbf916300411602",
            "0x32bf1451a3e999b57822bc1a9b8bfdfeb0da59aa330c247e4befafa997a11de9",
            "27"
        ],
        "from": "0x2173fdd5427c99357ba0dd5e34c964b08079a695"
    }
    ```

- 사용자 정의 API를 사용하는 경우 컨트랙트에서 `executeMetaTransaction` 함수를 실행합니다:

    (ref: [https://github.com/nglglhtr/ETHOnline-Workshop/blob/6b615b8a4ef00553c17729c721572529303c8e1b/2-network-agnostic-transfer/server/index.js#L40](https://github.com/nglglhtr/ETHOnline-Workshop/blob/6b615b8a4ef00553c17729c721572529303c8e1b/2-network-agnostic-transfer/server/index.js#L40))

    ```jsx
    try {
        let tx = await contract.methods.executeMetaTransaction(
          txDetails.from, txDetails.fnSig, r, s, v
        ).send ({
          from: user,
          gas: 800000
        })
        req.txHash = tx.transactionHash
      } catch (err) {
        console.log (err)
        next(err)
      }
    ```

    biconomy를 사용하는 경우 클라이언트 측 호출은 다음과 같습니다:

    ```jsx
    // client/src/App.js
    import React from "react";
    import Biconomy from "@biconomy/mexa";

    const getWeb3 = new Web3(biconomy);
    biconomy
        .onEvent(biconomy.READY, () => {
          // Initialize your dapp here like getting user accounts etc
          console.log("Mexa is Ready");
        })
        .onEvent(biconomy.ERROR, (error, message) => {
          // Handle error while initializing mexa
                console.error(error);
        });

    /**
    * use the getWeb3 object to define a contract and calling the function directly
    */

    ```
