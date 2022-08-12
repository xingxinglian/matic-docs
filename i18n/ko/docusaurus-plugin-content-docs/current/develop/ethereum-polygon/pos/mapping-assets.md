---
id: mapping-assets
title: POS를 이용한 자산 매핑
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

### 소개

이더리움과 폴리곤 간에 자산을 전송하려면 매핑이 필요합니다.

- **루트 체인** :: Goerli 또는 이더리움 메인넷을 나타냅니다.
- **하위 체인** :: 폴리곤 뭄바이 또는 폴리곤 메인넷을 나타냅니다.

루트 체인에 이미 토큰 컨트랙트가 배포되어 있고 이를 하위 체인으로 이동하려는 경우 이 설명을 따라야 하지만, 먼저 폴리곤 메인넷에 컨트랙트를 배포하려는 경우, 우선 하위 체인에서 토큰을 발행하고 그런 다음 루트 체인으로 다시 이동합니다. 그런 다음 이 [가이드](https://docs.polygon.technology/docs/develop/ethereum-polygon/mintable-assets)를 따라야 합니다.

## 표준 하위 토큰

표준 ERC20/ERC721/ERC1155 컨트랙트만 필요한 경우, https://mapper.polygon.technology/ 에서 매핑 요청을 제출하면 표준 하위 토큰 컨트랙트가 자동으로 배포됩니다.

표준 하위 토큰 컨트랙트는 다음과 같습니다:-
1. [ERC20](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildERC20.sol#L1492-#L1508)
2. [ERC721](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildERC721.sol#L2157-#L2238)
3. [ERC1155](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildERC1155.sol#L1784-#L1818)

새 매핑 요청을 만드는 방법을 이해하려면 이 [링크](/docs/develop/ethereum-polygon/submit-mapping-request)를 방문하세요.

## 맞춤형 하위 토큰

표준 함수에 추가 함수가 있는 맞춤형 하위 토큰 컨트랙트가 필요한 경우, **토큰 컨트랙트를 하위 체인에 배포**하고 [여기](https://mapper.polygon.technology/)에 매핑 요청을 제출하고 배포된 하위 토큰 컨트랙트의 주소를 포함해야 합니다. 맞춤형 하위 토큰 컨트랙트를 만드는 예를 설명하겠습니다.

**맞춤형 하위 컨트랙트는 하위 체인에 배포하기 전에 특정 지침을 따라야 합니다.**

`deposit` 메소드는 맞춤형 하위 토큰 컨트랙트에 있어야 합니다. 이 함수는 루트 체인에서 입금이 시작될 때마다 `ChildChainManagerProxy` 컨트랙트에 의해 호출됩니다. 이 deposit 함수는 내부적으로 하위 체인에 토큰을 발행합니다.

`withdraw` 메소드는 맞춤형 하위 토큰 컨트랙트에 있어야 합니다. 하위 체인에서 토큰을 소각하기 위해 호출할 수 있습니다. 소각은 출금 절차의 첫 번째 단계입니다. 이 withdraw함수는 내부적으로 하위 체인의 토큰을 소각합니다.

두 체인 간의 자산 균형을 적절하게 유지하려면 이러한 규칙을 따라야 합니다.

> 참고: 하위 토큰 컨트랙트의 constructor에는 토큰 발행이 없습니다.

#### 구현

하위 토큰 컨트랙트에서 `deposit` 및 `withdraw` 함수를 구현해야 하는 _이유_를 알아보았으므로 이제 구현을 진행할 수 있습니다.

```js title="ChildERC20.sol"
pragma solidity 0.6.6;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/math/SafeMath.sol";

contract ChildERC20 is ERC20,
{
    using SafeMath for uint256;

    constructor(string memory name, string memory symbol, uint8 decimals) public ERC20(name, symbol) {

        _setupDecimals(decimals);
        // can't mint here, because minting in child chain smart contract's constructor not allowed
        // _mint(msg.sender, 10 ** 27);

    }

    function deposit(address user, bytes calldata depositData) external {
        uint256 amount = abi.decode(depositData, (uint256));

        // `amount` token getting minted here & equal amount got locked in RootChainManager
        _totalSupply = _totalSupply.add(amount);
        _balances[user] = _balances[user].add(amount);

        emit Transfer(address(0), user, amount);
    }

    function withdraw(uint256 amount) external {
        _balances[msg.sender] = _balances[msg.sender].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);

        emit Transfer(msg.sender, address(0), amount);
    }

}
```

위의 코드 샘플에서 알 수 있는 한 가지는 `deposit` 함수는 누구나 호출할 수 있다는 것이나, 이는 허용되지 않습니다. 이를 방지하기 위해 `ChildChainManagerProxy`에서만 호출할 수 있도록 하고자 합니다. (ChildChainManagerProxy - on [Mumbai](https://mumbai.polygonscan.com/address/0xb5505a6d998549090530911180f38aC5130101c6/transactions) , on [Polygon Mainnet](https://polygonscan.com/address/0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa/) )

```js title="ChildERC20.sol"
pragma solidity 0.6.6;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/math/SafeMath.sol";

contract ChildERC20 is ERC20,
{
    using SafeMath for uint256;
    // keeping it for checking, whether deposit being called by valid address or not
    address public childChainManagerProxy;
    address deployer;

    constructor(string memory name, string memory symbol, uint8 decimals, address _childChainManagerProxy) public ERC20(name, symbol) {

        _setupDecimals(decimals);
        childChainManagerProxy = _childChainManagerProxy;
        deployer = msg.sender;

        // Can't mint here, because minting in child chain smart contract's constructor not allowed
        //
        // In case of mintable tokens it can be done, there can be external mintable function too
        // which can be called by some trusted parties
        // _mint(msg.sender, 10 ** 27);

    }

    // being proxified smart contract, most probably childChainManagerProxy contract's address
    // is not going to change ever, but still, lets keep it 
    function updateChildChainManager(address newChildChainManagerProxy) external {
        require(newChildChainManagerProxy != address(0), "Bad ChildChainManagerProxy address");
        require(msg.sender == deployer, "You're not allowed");

        childChainManagerProxy = newChildChainManagerProxy;
    }

    function deposit(address user, bytes calldata depositData) external {
        require(msg.sender == childChainManagerProxy, "You're not allowed to deposit");

        uint256 amount = abi.decode(depositData, (uint256));

        // `amount` token getting minted here & equal amount got locked in RootChainManager
        _totalSupply = _totalSupply.add(amount);
        _balances[user] = _balances[user].add(amount);

        emit Transfer(address(0), user, amount);
    }

    function withdraw(uint256 amount) external {
        _balances[msg.sender] = _balances[msg.sender].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);

        emit Transfer(msg.sender, address(0), amount);
    }

}
```

이 업데이트된 구현은 매핑에 사용할 수 있습니다.

단계 :

1. 루트 체인(예: {Goerli, Ethereum Mainnet})에 루트 토큰 배포합니다.
2. 하위 토큰에 `deposit`  및 `withdraw`함수가 있는지 확인합니다.
3. 하위 체인(예: {Polygon Mumbai, Polygon Mainnet})에 하위 토큰을 배포합니다.
4. 팀에서 해결할 매핑 요청을 제출합니다.

### 요청 제출

매핑 요청을 제출하려면 [이 링크](/docs/develop/ethereum-polygon/submit-mapping-request)를 사용하세요.
