---
id: full-node-deployment
title: 풀 노드 설정하기
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import useBaseUrl from '@docusaurus/useBaseUrl';

<Tabs
  defaultValue="mainnet"
  values={[
    { label: 'Polygon-Mainnet', value: 'mainnet', },
 { label: 'Polygon-Testnet', value: 'mumbai', },
 ]
}>
<TabItem value="mumbai">

## 전제조건

### 최소 시스템 요구사항

- 최소 시스템 요구사항은 다음과 같습니다:

    16 ~ 32 GiB의 메모리

    4 ~ 8 core CPU (t3 xLarge)

    최소 650GB SSD (확장가능여부 확인)

# 풀 노드 배포 (뭄바이 테스트넷)

전체 노드를 설정하기 위해 간단한 Ansible 플레이북을 만들었습니다.

전제조건:

- Ansible은 Python3.x.가 있는 로컬 장비에 설치해야 합니다. Python2.x.가 있는 경우 설정은 작동하지 않습니다.
    - Python 3.x가 있는 Ansible을 설치하려면 'pip3 install ansible' 명령을 사용할 수 있습니다. 그러면 Python 3 종속물들도 설치됩니다.
- 요구 사항은 [https://github.com/maticnetwork/node-ansible#requirements](https://github.com/maticnetwork/node-ansible#requirements)를 확인하세요.
- VM/머신에 Go가 설치되어 있지 않은지 확인해야 합니다. Go가 이미 설치되어 있는 경우 Asible을 통해 풀 노드를 설정하면 문제가 발생합니다. Go의 특정 패키지를 설치해야 하기 때문입니다.
- 또한 VM/머신에 Polygon Validator 또는 Heimdall 또는 Bor에 대한 이전 설정이 없는지 확인해야 합니다. 설정에 문제가 발생하므로 삭제해야 합니다.
- 최신 버전인, [Heimdall v.0.2.9](https://github.com/maticnetwork/heimdall/releases/tag/v0.2.9)에는 **체인의 진행을 방해할 수 있는 갑작스러운 이벤트의 경우에 mempool이 매우 빨리 채워지지 않도록 서로 다른 validator들의 컨트랙트 이벤트간 지연시간이 증가하는 것 같은 몇가지 개선 사항이 포함되어 있습니다.  또한 데이터 크기는 상태 동기화 트랜잭션에서 30Kb(byte로 표시되는 경우) 및 60Kb(문자열로 표시되는 경우)로 제한됩니다**. 예를들어:
```
Data - "abcd1234"
Length in string format - 8
Hex Byte representation - [171 205 18 52]
Length in byte format - 4
```
Testnetv4/뭄바이 테스트넷을 위한 풀 노드 설치

- 풀 노드가 설치되는 원격장비나 VM에 대한 액세스 권한이 있는지 확인합니다. 자세한 내용은 [https://github.com/maticnetwork/node-ansible#setup](https://github.com/maticnetwork/node-ansible#setup) 을 참조하시기 바랍니다.
- [`https://github.com/maticnetwork/node-ansible`](https://github.com/maticnetwork/node-ansible) 리포지토리를 복제합니다.
- 'cd node-ansible'
- 'inventory.yml' 파일을 편집하고  'sentry->hosts' 섹션에 IP를 삽입합니다. 자세한 내용은 [https://github.com/maticnetwork/node-ansible#inventory](https://github.com/maticnetwork/node-ansible#inventory)를 참조하십시오.
- 'ansible sentry -m ping' 를 실행하여 원격장비에 연결할 수 있는지 확인하십시오.
- 올바른 원격장비/VM이 구성되었는지 확인하기 위한 테스트 실행을 위하 다음 명령을 실행합니다:

    `ansible-playbook -l sentry playbooks/network.yml --extra-var="bor_branch=v0.2.16 heimdall_branch=v0.2.9 network_version=testnet-v4 node_type=sentry/sentry heimdall_network=mumbai" --list-hosts`


    구성한 원격장비의 IP를 출력해야 합니다.

    <img src={useBaseUrl("img/network/full-node-mumbai.png")} />

- 다음 명령으로 풀 노드를 설정합니다:
    `ansible-playbook -l sentry playbooks/network.yml --extra-var="bor_branch=v0.2.16 heimdall_branch=v0.2.9 network_version=testnet-v4 node_type=sentry/sentry heimdall_network=mumbai"`

- 문제가 발생하면, 다음을 사용하여 전체 설정을 삭제하고 정리하십시오.
    `ansible-playbook -l sentry playbooks/clean.yml`

- 원격 장비에 로그인 합니다.
- '~/.heimdalld/config/config.toml' 에 다음을 구성합니다:
    - `moniker=<enter unique identifier>`
    - `seeds="4cd60c1d76e44b05f7dfd8bab3f447b119e87042@54.147.31.250:26656"`
- `~/.heimdalld/config/heimdall-config.toml`에 다음을 구성합니다:
    - `eth_rpc_url =<insert Infura or any full node RPC URL to Goerli>`
- `~/node/bor/start.sh`에서 다음 플래그를 `bor` 시작 매개변수에 추가합니다:

```bash
--bootnodes "enode://320553cda00dfc003f499a3ce9598029f364fbb3ed1222fdc20a94d97dcc4d8ba0cd0bfa996579dcc6d17a534741fb0a5da303a90579431259150de66b597251@54.147.31.250:30303"
```

- Bor에 대한 `trace`를 켜려면켜려면 '~/node/bor/start.sh:' 에서 'bor' 시작 매개변수에 다음 플래그를 추가하십시오.:
    - `--gcmode 'archive'`

- 다음 명령들로 풀 노드를 실행합니다:
     - `sudo service heimdalld start`
    - `sudo service heimdalld-rest-server start`

    일단Heimdall이 동기화되면, 다음을 실행합니다.

    - `sudo service bor start`

- 로그 체크하기:
    - Heimdall - `journalctl -u heimdalld.service -f`
    - Heimdall Rest Server - `journalctl -u heimdalld-rest-server.service -f`
    - Bor - `journalctl -u bor.service -f`

- Heimdall 이 동기화되었는지 체크하기위해
    - 원격 장비/VM에서 'curl localhost:26657/status'을 실행합니다.
    - 결과에서, 'catching_up'값이 'false'여야 합니다.

</TabItem>
<TabItem value="mainnet">

## 전제조건

### 최소 시스템 요구사항

- 최소 시스템 요구사항은 다음과 같습니다:

    16 ~ 32 GiB의 메모리

    4 ~ 8 core CPU (t3 xLarge)

    최소 650GB SSD (확장가능여부 확인)

# 풀 노드 배포 (폴리곤 메인넷)
전체 노드를 설정하기 위해 간단한 Ansible 플레이북을 만들었습니다.

전제조건:

- Ansible은 Python3.x.가 있는 로컬 장비에 설치해야 합니다. Python2.x.가 있는 경우 설정은 작동하지 않습니다.
    - Python 3.x가 있는 Ansible을 설치하려면 'pip3 install ansible' 명령을 사용할 수 있습니다. 그러면 Python 3 종속물들도 설치됩니다.
- 자세한 내용은 [https://github.com/maticnetwork/node-ansible#requirements](https://github.com/maticnetwork/node-ansible#requirements) 을 참조하시기 바랍니다.
- VM/머신에 Go가 설치되어 있지 않은지 확인해야 합니다. Go가 이미 설치되어 있는 경우 ansible을 통해 풀 노드를 설정하면 문제가 발생합니다. Go의 특정 패키지를 설치해야 하기 때문입니다.
- 또한 VM/머신에 Polygon Validator 또는 Heimdall 또는 Bor에 대한 이전 설정이 없는지 확인해야 합니다. 설정에 문제가 발생하므로 삭제해야 합니다.
- 최신 버전인, [Heimdall v.0.2.9(https://github.com/maticnetwork/heimdall/releases/tag/v0.2.9)에는 **체인의 진행을 방해할 수 있는 갑작스러운 이벤트의 경우에 mempool이 매우 빨리 채워지지 않도록 서로 다른 validator들의 컨트랙트 이벤트간 지연시간이 증가하는 것 같은 몇가지 개선 사항이 포함되어 있습니다.  또한 데이터 크기는 상태 동기화 트랜잭션에서 30Kb(byte로 표시되는 경우) 및 60Kb(문자열로 표시되는 경우)로 제한됩니다**. 예를들어:

```
Data - "abcd1234"
Length in string format - 8
Hex Byte representation - [171 205 18 52]
Length in byte format - 4
```
폴리곤 메인넷을 위한 풀 노드 설치

- 풀 노드가 설치되는 원격장비나 VM에 대한 액세스 권한이 있는지 확인합니다. 자세한 내용은  [https://github.com/maticnetwork/node-ansible#setup](https://github.com/maticnetwork/node-ansible#setup) 을 참조하시기 바랍니다.
- [`https://github.com/maticnetwork/node-ansible`](https://github.com/maticnetwork/node-ansible) 리포지토리를 복제합니다.
- 'cd node-ansible'
- 'inventory.yml' 파일을 편집하고  'sentry->hosts' 섹션에 IP를 삽입합니다. 자세한 내용은 [https://github.com/maticnetwork/node-ansible#inventory](https://github.com/maticnetwork/node-ansible#inventory)를 참조하십시오.
- 'ansible sentry -m ping' 를 실행하여 원격장비에 연결할 수 있는지 확인하십시오.
- 올바른 원격장비/VM이 구성되었는지 확인하기 위한 테스트 실행을 위하 다음 명령을 실행합니다:

    `ansible-playbook -l sentry playbooks/network.yml --extra-var="bor_branch=v0.2.16 heimdall_branch=v0.2.9 network_version=mainnet-v1 node_type=sentry/sentry heimdall_network=mainnet" --list-hosts`


    구성한 원격장비의 IP를 출력해야 합니다.

    <img src={useBaseUrl("img/network/full-node-mainnet.png")} />

- 다음 명령으로 풀 노드를 설정합니다:
    `ansible-playbook -l sentry playbooks/network.yml --extra-var="bor_branch=v0.2.16 heimdall_branch=v0.2.9 network_version=mainnet-v1 node_type=sentry/sentry heimdall_network=mainnet"`

- 문제가 발생하면, 다음을 사용하여 전체 설정을 삭제하고 정리하십시오.
    `ansible-playbook -l sentry playbooks/clean.yml`

- 원격 장비에 로그인 합니다.
- '~/.heimdalld/config/config.toml' 에 다음을 구성합니다:
    - `moniker=<enter unique identifier>`
    - `seeds="f4f605d60b8ffaaf15240564e58a81103510631c@159.203.9.164:26656,4fb1bc820088764a564d4f66bba1963d47d82329@44.232.55.71:26656,2eadba4be3ce47ac8db0a3538cb923b57b41c927@35.199.4.13:26656,3b23b20017a6f348d329c102ddc0088f0a10a444@35.221.13.28:26656,25f5f65a09c56e9f1d2d90618aa70cd358aa68da@35.230.116.151:26656"`
- `~/.heimdalld/config/heimdall-config.toml`에서 다음을 구성합니다:
    - `eth_rpc_url =<insert Infura or any full node RPC URL to Ethereum>`
- `~/node/bor/start.sh`에서 다음 플래그를 `bor` 시작 매개변수에 추가합니다:

```bash
--bootnodes "enode://0cb82b395094ee4a2915e9714894627de9ed8498fb881cec6db7c65e8b9a5bd7f2f25cc84e71e89d0947e51c76e85d0847de848c7782b13c0255247a6758178c@44.232.55.71:30303,enode://88116f4295f5a31538ae409e4d44ad40d22e44ee9342869e7d68bdec55b0f83c1530355ce8b41fbec0928a7d75a5745d528450d30aec92066ab6ba1ee351d710@159.203.9.164:30303"
```

- Bor에 대한 `trace`를 켜려면켜려면 '~/node/bor/start.sh:' 에서 'bor' 시작 매개변수에 다음 플래그를 추가하십시오.:
    - `--gcmode 'archive'`

- 다음 명령들로 풀 노드를 실행합니다:
     - `sudo service heimdalld start`
    - `sudo service heimdalld-rest-server start`

    일단Heimdall이 동기화되면, 다음을 실행합니다.

    - `sudo service bor start`

- 로그 체크하기:
    - Heimdall - `journalctl -u heimdalld.service -f`
    - Heimdall Rest Server - `journalctl -u heimdalld-rest-server.service -f`
    - Bor - `journalctl -u bor.service -f`

- Heimdall 이 동기화되었는지 체크하기위해
    - 원격 장비/VM에서 'curl localhost:26657/status'을 실행합니다.
    - 결과에서, 'catching_up'값이 'false'여야 합니다.

</TabItem>
</Tabs>
