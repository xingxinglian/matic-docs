---
id: custom-tokens
title: 맞춤형 토큰 구성하기
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---
import useBaseUrl from '@docusaurus/useBaseUrl';

이 페이지는 메타마스크에 맞춤형 토큰을 설정/추가하는 과정을 보여줍니다. 특히, 우리는 예제  `TEST` ERC20 과 ERC721 토큰을 Görli 테스트넷과 폴리곤 테스트넷인 Mumbai에 추가하는 것을 시연하였습니다.

이 프로세스를 사용하여 사용자 정의 ERC20 토큰을 메타마스크의 모든 네트워크에 추가할 수 있습니다.

**Görli 네트워크의 메타마스크 계정에 `TEST` 토큰(ERC20) 추가하기**

Görli 네트워크의 계정에 `TEST` 토큰을 표시하려면 메타마스크에서 Add Tokens(토큰 가져오기) 옵션을 클릭하면 됩니다. 그런 다음 화면으로 이동하여 Custom Token(맞춤형 토큰)탭을 클릭하고 토큰 주소 필드에 아래 주소를 복사하여 붙여 넣습니다

Görli의 `TEST`토큰의 컨트랙트 주소는`0x3f152B63Ec5CA5831061B2DccFb29a874C317502`입니다. `TEST` 토큰은 설명을 위해 폴리곤 개발자 문서 전체에서 사용되는 예시ERC20 토큰 컨트랙트입니다.

다른 필드는 자동으로 채워집니다. 저장을 클릭한 다음 Add Tokens(토큰 가져오기)를 클릭하십시오. 이제 `TEST`토큰이 메타마스크의 계정에 표시됩니다ㅓ.

**`Matic TST` 토큰을 메타마스크에 구성하기**

또한 입문 Matic.js 튜토리얼을 따르는 경우 시각화를 위해 폴리곤의 테스트넷에 `TST` 토큰을 구성해야 합니다. **Polygon 테스트넷 을 가리키도록 메타마스크의 네트워크를 전환합니다 - https://rpc-mumbai.matic.today**. 메타마스크에서 이것은 `Private Network`로서 또는 맞춤형 rpc를 추가할 때 이름을 지정한 대로 표시됩니다.(예: `mumbai`).

폴리곤 테스트넷의 해당 `TST` 토큰 주소는 `0x2d7882beDcbfDDce29Ba99965dd3cdF7fcB10A1e`입니다. 이 토큰 컨트랙트 주소는 Goerli의 주소와 다릅니다. 이는 폴리곤 네트워크의 `TST`토큰이기 때문입니다. 사용자 지정 토큰을 추가하기 위한 자세한 화면별 가이드는 다음과 같습니다:

메타마스크를 연 다음 Add Token(토큰가져오기) 옵션을 클릭할 수 있습니다.

<img src={useBaseUrl("img/metamask/configure-custom-token-1.png")} />

이미 사용 가능한 토큰 목록에서 검색하거나 맞춤형 토큰을 추가하는 화면이 표시됩니다. 맞춤형 토큰(Custom Token)을 클릭하십시오.

토큰 주소를 추가하는 필드가 표시됩니다. 양식에 토큰 주소를 붙여 넣고 토큰 이름을 `TST`로 구성합니다.

<img src={useBaseUrl("img/metamask/configure-custom-token-2.png")} />

Next를 클릭합니다.

<img src={useBaseUrl("img/metamask/configure-custom-token-3.png")} />

그런 다음 토큰 추가(Add Toekn)를 클릭합니다. 홈 화면으로 돌아가면 새 토큰이 토큰 목록에 표시됩니다.

**Görli 네트워크의 메타마스크 계정에 `ERC721-TESTV4` 토큰(ERC721) 추가하기**

Görli 네트워크의 계정에 `ERC721-TESTV4`토큰을 표시하려면 메타마스크에서 토큰 추가(Add Token) 옵션을 클릭하면 됩니다. 그런 다음 화면으로 이동하여 Custom Token(맞춤형 토큰)탭을 클릭하고 토큰 주소 필드에 아래 주소를 복사하여 붙여 넣습니다

Görli의 `ERC721-TESTV4`토큰 컨트랙트 주소는 `0xfA08B72137eF907dEB3F202a60EfBc610D2f224b`입니다.  `ERC721-TESTV4`토큰은 예시 ERC721 토큰 컨트랙트입니다.

토큰 기호는 `ERC721-Testv4`이고 토큰 십진수는 `18`입니다. 토큰 추가(Add Token)를 클릭합니다. `ERC721-TESTV4` 토큰은 이제 메타마스크의 계정에 표시됩니다

**뭄바이 네트워크의 메타마스크 계정에 `ERC721-TESTV4`토큰(ERC721) 추가하기**

**메타마스크의 네트워크를 폴리곤 테스트넷으로 전환합니다 - https://rpc-mumbai.matic.today**. 메타마스크에서 이것은 `Private Network`로서 또는 맞춤형 rpc를 추가할 때 이름을 지정한 대로 표시됩니다.(예: `mumbai`).

뭄바이 네트워크의 계정에 `ERC721-TESTV4`토큰을 표시하려면 메타마스크에서 토큰 추가(Add Token) 옵션을 클릭하면 됩니다. 그런 다음 화면으로 이동하여 Custom Token(맞춤형 토큰)탭을 클릭하고 토큰 주소 필드에 아래 주소를 복사하여 붙여 넣습니다

뭄바이의 `ERC721-TESTV4`토큰 컨트랙트 주소는 `0x33FC58F12A56280503b04AC7911D1EceEBcE179c`입니다. `ERC721-TESTV4`토큰은 예시 ERC721 토큰 컨트랙트입니다.

토큰 기호는 `ERC721-Testv4`이고 토큰 십진수는 `18`입니다. 토큰 추가(Add Token)를 클릭합니다. `ERC721-TESTV4` 토큰은 이제 메타마스크의 계정에 표시됩니다

**메타마스크 계정에 테스트 ERC1155 토큰 추가하기**

폴리곤 네트워크는 ERC1155를 지원하지만, [메타마스크는 아직 표준을 지원하지 않습니다. 이 업데이트는 2021년 4분기로 예상됩니다](https://metamask.zendesk.com/hc/en-us/articles/360058488651-Does-MetaMask-support-ERC-1155-). 이 업데이트는 2021년 4분기로 예상됩니다.
