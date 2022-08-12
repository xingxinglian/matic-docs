---
id: widget
title: 지갑 위젯
sidebar_label: 지갑 위젯
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

지갑 위젯은 입금 및 출금과 같은 브리지 트랜잭션을 실행하기 위해 모든 웹 애플리케이션에 포함될 수 있는 UI 도구입니다.

모든 위젯은 [위젯 대시보드](https://wallet.polygon.technology/widget-dashboard)에서 얻을 수 있는 고유한 이름으로 식별됩니다.

### 위젯 대시보드

위젯은 지갑 애플리케이션의 위젯 대시보드 페이지에서 생성할 수 있습니다. 그것은 사용자가 몇 가지 사용자 정의 옵션으로 새 위젯을 만들 수 있습니다.

위젯이 생성되면 코드 조각을 복사하여 애플리케이션에 추가하거나 위젯 이름을 사용하여 직접 구성할 수 있습니다.

위젯 대시보드 링크-

* 메인넷 - https://wallet.polygon.technology/widget-dashboard
* 테스트넷 - https://wallet-dev.polygon.technology/widget-dashboard

## 설치

위젯은 자바스크립트 라이브러리로 내보내지고 npm 패키지로 제공됩니다.

```bash 
npm i @maticnetwork/wallet-widget
```

## 예시

개발에 도움이 되는 다양한 프레임워크 및 도구에 대한 예제를 만들었습니다. 모든 예제는 여기에 있습니다. - [https://github.com/maticnetwork/wallet-widget-example](https://github.com/maticnetwork/wallet-widget-example)

## 사용법
### 목표를 갖고

앱에 버튼이 있고 해당 버튼을 클릭할 때 위젯을 표시하고 싶다고 가정해 보십시오-

```html
<button id="btnMaticWidget"></btn>
```

```javascript
import { Widget } from "@maticnetwork/wallet-widget";

var widget = new Widget({
    appName: "<widget name>", //widget name from dashboard
    target: '#btnMaticWidget', // element selector for showing widget on click
    network: 'mainnet' // network to be used - testnet or mainnet
});
```

준비가 되면 위젯을 만듭니다. 문서가 로드된 후 create 함수를 호출하는 것이 가장 좋습니다.

```javascript 
await widget.create();
```
위젯이 생성되었으므로 이제 버튼을 클릭하면 위젯이 표시됩니다.

### 목표 없이

```javascript
import { Widget } from "@maticnetwork/wallet-widget";

var widget = new Widget({
    appName: "<widget name>", //widget name from dashboard
    network: 'mainnet' // network to be used - testnet or mainnet
});

await widget.create();
```

이제 위젯이 생성되었지만 위젯을 표시하려면 `show` API를 호출해야 합니다.

```
widget.show();
```

마찬가지로 `hide` API를 호출하여 위젯을 숨길 수 있습니다.

```
widget.hide();
```

### 중요사항👉

1. 네트워크 "testnet" 또는 "mainnet"을 기반으로 각 대시보드에서 앱을 생성해야 합니다. 네트워크 변경 시 문제가 없도록 테스트넷과 메인넷 모두 동일한 이름으로 앱을 생성하는 것을 권장합니다.

2. 지갑 위젯은 UI 라이브러리이며 다른 웹사이트에서는 다르게 보일 수 있으며 색상, 응답성 등과 같은 문제가 있을 수 있습니다. 따라서 테스트 및 사용자 지정에 시간을 할애하십시오. 도움이 필요한 경우 [지원팀](https://support.polygon.technology/)에 문의하세요.

3. 지갑 위젯은 모바일 기기에서 전체 화면으로 표시되지만 `스타일` 구성에 따라 사용자 정의할 수 있습니다.

## 구성

Widget constructor에서 구성을 제공할 수 있습니다.

## 사용 가능한 구성은

- **target** : string - 요소 클릭 시 위젯을 표시하기 위한 CSS 선택기. 예를 들어 "#btnMaticWidget"은 아래 코드의 대상이 됨.

```javascript
<button id="btnMaticWidget">Matic widget</button>
```

- **network** : string - 사용할 네트워크. 'testnet' 또는 'mainnet'의 두 가지 옵션을 사용할 수 있음.
- **width** : number – 위젯의 너비
- **height** : number – 위젯의 높이
- **autoShowTime** : number - 지정된 시간(밀리초) 후에 위젯을 자동으로 표시.
- **appName** : string - 앱의 이름, 이것으로 위젯 대시보드에서 검색할 수 있음.
- **position** : 문자열 - 위젯의 위치를 설정함. 사용 가능한 옵션은-
    - center
    - bottom-right
    - bottom-left
- **amount** : 문자열 - 텍스트 상자에 금액을 미리 채움
- **page** : 문자열 - 페이지를 선택함. 사용 가능한 옵션은 - `withdraw`, `deposit`.
- **overlay** : 불린 - 위젯이 열릴 때 오버레이를 표시함. 기본적으로 false임.
- **style** : object - 위젯에 일부 CSS 스타일을 적용.

```
var widget = new MaticWidget({
    appName: "<your app id>", //appName from dashboard
    target: '#btnMaticWidget', // element selector for showing widget on click
    network: 'testnet' // network to be used - testnet or mainnet,
    style:{
      color:'red'
    }
});
```

## 이벤트

위젯은 애플리케이션 내부에서 무슨 일이 일어나고 있는지 아는 데 사용할 수 있는 몇 가지 이벤트를 내보냅니다.

### 이벤트 구독

```javascript
widget.on('load',()=>{
  console.log('widget is loaded');
})
```

### 이벤트 구독 취소

```javascript 
widget.off('load',<callback>)
```

> 콜백은 이벤트를 구독하는 데 사용된 것과 동일해야 합니다. 따라서 변수에 콜백을 저장하는 것이 좋습니다.'

## 이벤트 목록:

- **load** – 위젯이 로드됨
- **close** - 위젯이 닫힘
- **approveInit** - 승인 트랜잭션이 초기화됨
- **approveComplete** - 승인 트랜잭션이 완료됨
- **approveError** - 일부 오류로 인해 승인 트랜잭션이 실패했거나 사용자가 메타마스크에서 트랜잭션을 거부함
- **depositInit** - 입금 트랜잭션이 초기화됨
- **depositComplete** - 입금 트랜잭션이 완료됨
- **depositError** -어떤 오류로 인해 입금 거래가 실패했거나 사용자가 메타마스크에서 입금 완료 트랜잭션을 거부함
- **burnInit** - 출금 소각 트랜잭션이 초기화됨
- **burnComplete** - 출금 소각 트랜잭션이 완료됨
- **confirmWithdrawInit** - 출금이 확인되고 트랜잭션이 초기화되었는지 확인
- **confirmWithdrawComplete** - 완료된 출금 확인 트랜잭션
- **confirmWithdrawError** - 일부 오류로 인해 출금 확인 거래가 실패했거나 사용자가 메타마스크에서 출금 확인 거래를 거부
- **exitInit** - 출금 종료 트랜잭션이 초기화됨
- **exitComplete** - 출금 종료 트랜잭션이 완료됨
- **exitError** - 일부 오류로 인해 출금 종료 트랜잭션이 실패했거나 사용자가 메타마스크에서 출금 종료 트랜잭션을 거부

## APIS

- **show** - 위젯을 보여줌

```javascript
widget.show()
```

- **hide** - 위젯을 숨김

```javascript
widget.hide()
```

- **on** - 이벤트 구독

```javascript
widget.on('<event name>', callback)
```

- **off** - 이벤트 구독 취소

```javascript
widget.off('<event name>', callback)
```
