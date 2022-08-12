---
id: full-node-binaries
title: 풀 노드 바이너리
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

'@theme/Tabs'로부터 Tabs 가져오기; '@theme/TabItem'로부터 TabItem 가져오기; '@docusaurus/useBaseUrl'로부터 useBaseUrl 가져오기;

<Tabs
  defaultValue="mainnet"
  values={[
    { label: 'Polygon-Mainnet', value: 'mainnet', },
 { label: 'Mumbai-Testnet', value: 'mumbai', },
 ]
}>

<TabItem value="mumbai">

이 섹션에서는 바이너리에서 풀 노드를 시작하고 실행하는 방법을 안내합니다.

시스템 요구사항은 [Minimum Technical Requirements](http://localhost:3000/docs/develop/network-details/technical-requirements)을 참조하십시오.

:::참고
이 가이드의 단계들에는 Heimdall 및 Bor 서비스가 완전히 동기화될 때까지 기다리는 작업이 포함됩니다. 이 프로세스를 완료하는 데 며칠이 걸립니다.

아니면 유지 관리되는 스냅샷을 사용하여 동기화 시간을 몇 시간으로 줄일 수 있습니다. 자세한 지침은 [Snapshot Instructions for Heimdall and Bor](https://forum.matic.network/t/snapshot-instructions-for-heimdall-and-bor/2278)을 참조하십시오.

스냅샷 다운로드 링크는 [Polygon Chains Snapshots](https://snapshots.matic.today/)을 참조하십시오.

## 전제조건


- 하나의 머신이 필요함
풀 노드 머신에 설치된 'build-essential'
- 설치를 위해: 
- 'sudo apt-get install build-essential'
- 양쪽 풀 노드 머신에 설치된 Go 1.17<!-- ### To install

```bash wget https://gist.githubusercontent.com/ssandeep/a6c7197811c83c71e5fead841bab396c/raw/go-install.sh
```

```bash
go-install.sh
```

```bash
sudo ln -nfs ~/.go/bin/go /usr/bin/go
``` -->

<!-- RabbitMQ installed on both the Full Node machines. See Downloading and Installing RabbitMQ. -->


## 개요

- 머신 하나를 준비합니다.
- 풀 노드 머신에 Heimdal및 Bor 바이너리를 설치합니다
- 풀 노드 머신에서 Heimdall 및 Bor 서비스를 설정합니다.
- 풀 노드를 구성합니다.
- 풀 노드를 시작합니다.
- 커뮤니티와 함께 노드 상태를 확인합니다.

:::참고
정확한 순서를 따라야 합니다. 그렇지 않으면 문제가 발생하게 됩니다.
### build essentials 설치

```bash
sudo apt-get install build-essential
```

### **GO 설치**

```bash
wget https://gist.githubusercontent.com/ssandeep/a6c7197811c83c71e5fead841bab396c/raw/go-install.sh
bash go-install.sh
sudo ln -nfs ~/.go/bin/go /usr/bin/go
```

> 참고: 버전 1.17의 Go를 권장합니다.

### RabbitMq

RabbitMQ는 메시지 브로커 또는 대기열 관리자라고도 하는 메시지 큐잉 소프트웨어입니다. 간단히 말해서; 하나 또는 여러 메시지를 전송하기 위해 응용 프로그램이 연결하는 대기열이 정의된 소프트웨어입니다.

Heimdall 코드 베이스에 내장된 `bridge` 라는 도우미 서비스는 여러 네트워크에 트랜잭션을 대기열에 추가하기 위해 `rabbit-mq` 가 필요합니다. 설치는 매우 간단해야 합니다.

**여기 다운로드 지침을 확인하십시오: [https://www.rabbitmq.com/download.html](https://www.rabbitmq.com/download.html)**

```bash
rabbitmq-server
```

## 바이너리 설치하기

### Heimdall

다음으로, 최신 버전의 Heimdall을 설치합니다. Git에서 올바른 [배포 버전](https://github.com/maticnetwork/heimdall/releases)을 체크했는지 확인하십시오.

```bash
cd ~/
git clone https://github.com/maticnetwork/heimdall
cd heimdall

# Checkout to a proper version
# For eg: git checkout v0.2.1-mumbai
git checkout <TAG OR BRANCH>
make install
```

그러면 `heimdalld` 및 `heimdallcli` 바이너리가 설치됩니다. 모든 것이 정상인지 확인하십시오:

```bash
heimdalld version --long
```

### Bor

다음으로 최신 버전의 Bor를 설치합니다. Git에서 올바른 [배포 버전](https://github.com/maticnetwork/bor/releases)을 체크했는지 확인하십시오.

```bash
cd ~/
git clone https://github.com/maticnetwork/bor
cd bor

# Checkout to a proper version

# For eg: git checkout v0.2.16

git checkout <TAG OR BRANCH>
make bor-all
sudo ln -nfs ~/bor/build/bin/bor /usr/bin/bor
sudo ln -nfs ~/bor/build/bin/bootnode /usr/bin/bootnode
```

그러면 'bor' 바이너리와 'bootnode' 바이너리가 설치됩니다:

```bash
bor version
```

## 노드 파일 설정하기

### launch 리포지토리 가져오기

```bash
cd ~/
git clone https://github.com/maticnetwork/launch
```

### launch 디렉토리 설정하기

네트워크 디렉토리를 설정하려면 네트워크 이름과 노드 유형이 필요합니다.

사용 가능한 네트워크: `mainnet-v1` 및 `testnet-v4`

노드 유형: `sentry` 및 `validator`

```bash
cd ~/
mkdir -p node
cp -rf launch/<network-name>/sentry/<node-type>/* ~/node

# To setup sentry node for mumbai (testnet-v4) testnet
# cp -rf launch/testnet-v4/sentry/sentry/* ~/node
```

### 네트워크 디렉토리 구성하기

**Heimdall 데이터 설정**

```bash
cd ~/node/heimdall
bash setup.sh
```

**Bor 데이터 설정**

```bash
cd ~/node/bor
bash setup.sh
```

## 서비스 파일 구성하기

service.sh 파일 다운로드

```bash
cd ~/node
wget https://raw.githubusercontent.com/maticnetwork/launch/master/<network-name>/service.sh
# To setup sentry node for mumbai (testnet-v4) testnet
# wget https://raw.githubusercontent.com/maticnetwork/launch/master/testnet-v4/service.sh
```

메타데이터 파일 생성하기
```bash
sudo mkdir -p /etc/matic
sudo chmod -R 777 /etc/matic/
touch /etc/matic/metadata
```

서비스 파일을 생성하고 시스템 디렉토리에 복사하기

```bash
cd ~/node
bash service.sh
sudo cp *.service /etc/systemd/system/
```

## config 파일 설정하기

**뭄바이 테스트넷용**

- `~/.heimdalld/config/config.toml`에 다음을 구성합니다:
    - `moniker=<enter unique identifier>`

```js
 seeds="4cd60c1d76e44b05f7dfd8bab3f447b119e87042@54.147.31.250:26656,b18bbe1f3d8576f4b73d9b18976e71c65e839149@34.226.134.117:26656"
```
- `vi ~/.heimdalld/config/heimdall-config.toml`에 다음을 구성합니다:

    ```js
    eth_rpc_url =<insert Infura or any full node RPC URL to Goerli>
    ```

- `vi ~/node/bor/start.sh`의 다음 플래그를 `bor` 시작 매개변수에 추가합니다:

```bash
--bootnodes "enode://320553cda00dfc003f499a3ce9598029f364fbb3ed1222fdc20a94d97dcc4d8ba0cd0bfa996579dcc6d17a534741fb0a5da303a90579431259150de66b597251@54.147.31.250:30303"
```

## 서비스 시작하기

다음 명령을 사용하여 전체 Heimdall 노드를 실행합니다:

```bash
sudo service heimdalld start
sudo service heimdalld-rest-server start
```

일단 Heimdall이 동기화되면, Bor를 시작합니다:

```bash
sudo service bor start
```

## 로그

로그는 journalctl linux tool에의해 관리됩니다. 고급 사용을 위한 링크는 다음과 같습니다: [https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs)

**Heimdall 노드 로그 확인하기**

```bash
journalctl -u heimdalld.service -f
```

**Heimdall rest server 로그 확인하기**

```bash
journalctl -u heimdalld-rest-server.service -f
```

**Bor rest server 로그 확인하기**

```bash
journalctl -u bor.service -f
```

### Heimdall이 동기화 되었는지 확인하려면
1. 원격 머신 또는 VM에서, `curl localhost:26657/status`을 실행합니다.
2. 출력에서, `catching_up` 값은 `false`여야 합니다

### **포트 및 방화벽 설정하기**

센트리 노드 방화벽에서 포트 22, 26656 과 30303 을 월드(0.0.0.0/0)로 엽니다.

VPN을 사용하여 요구 사항 및 보안 지침에 따라 포트 22에 대한 액세스를 제한할 수 있습니다.


</TabItem>

<TabItem value="mainnet">

# 바이너리를 이용한 폴리곤 풀 노드 설정하기

이 섹션에서는 바이너리에서 풀 노드를 시작하고 실행하는 방법을 안내합니다.

시스템 요구사항은 [Minimum Technical Requirements](https://docs.polygon.technology/docs/develop/network-details/technical-requirements)을 참조하십시오.

:::참고
이 가이드의 단계들에는 Heimdall 및 Bor 서비스가 완전히 동기화될 때까지 기다리는 작업이 포함됩니다. 이 프로세스를 완료하는 데 며칠이 걸립니다.

아니면 유지 관리되는 스냅샷을 사용하여 동기화 시간을 몇 시간으로 줄일 수 있습니다. 자세한 지침은 [Snapshot Instructions for Heimdall and Bor](https://forum.matic.network/t/snapshot-instructions-for-heimdall-and-bor/2278)을 참조하십시오.

스냅샷 다운로드 링크는 [Polygon Chains Snapshots](https://snapshots.matic.today/)을 참조하십시오.

## 전제조건


- 하나의 머신이 필요함
풀 노드 머신에 설치된 'build-essential'
- 설치를 위해: 
- 'sudo apt-get install build-essential'
- 양쪽 풀 노드 머신에 설치된 Go 1.17

<!-- ### To install

```bash
wget https://gist.githubusercontent.com/ssandeep/a6c7197811c83c71e5fead841bab396c/raw/go-install.sh
```

```bash
go-install.sh
```

```bash
sudo ln -nfs ~/.go/bin/go /usr/bin/go
```

RabbitMQ installed on both the Full Node machines. See Downloading and Installing RabbitMQ. -->

Two machines — one local machine on which you will run the Ansible playbook; one 
remote machine — for Full Node.
- On the local machine, Ansible installed.
- On the local machine, Python 3.x installed.
- On the remote machine, make sure Go is not installed.
- On the remote machine, your local machine's SSH public key is on the remote machine to let Ansible connect to them. -->


## 개요

- 머신 하나를 준비합니다.
- 풀 노드 머신에 Heimdal및 Bor 바이너리를 설치합니다
- 풀 노드 머신에서 Heimdall 및 Bor 서비스를 설정합니다.
- 풀 노드를 구성합니다.
- 풀 노드를 시작합니다.
- 커뮤니티와 함께 노드 상태를 확인합니다.

:::참고
정확한 순서를 따라야 합니다. 그렇지 않으면 문제가 발생하게 됩니다.
### build essentials 설치하기

***이것은 풀 노드를 위해 필요합니다***

```bash
sudo apt-get install build-essential
```

### GO 설치하기

***이것은 풀 노드를 위해 필요합니다***

```bash
wget https://gist.githubusercontent.com/ssandeep/a6c7197811c83c71e5fead841bab396c/raw/go-install.sh
bash go-install.sh
sudo ln -nfs ~/.go/bin/go /usr/bin/go
```

> 참고: Go 버전 1.17을 권장합니다

## 바이너리 설치하기

### Heimdall

***이것은 풀 노드를 위해 필요합니다***

다음으로, 최신 버전의 Heimdall과 서비스를 설치합니다. Git에서 정식으로 [배포된 버전](https://github.com/maticnetwork/heimdall/releases)을 확인하십시오. 최신 버전인 [Heimdall v.0.2.9](https://github.com/maticnetwork/heimdall/releases/tag/v0.2.9)에는 다음과 같은 몇가지 개선사항이 포함되어 있습니다:
1. 상태 동기화 트랜잭션 크기를 다음으로 제한:
    * **bytes**로 표시되는 경우 **30Kb** 
    * **string**으로 표시되는 경우 **60Kb**.
2. 체인의 진행을 방해할 수 있는 갑작스러운 이벤트의 경우 mempool이 매우 빠르게 채워지지 않도록 다른 validators의 컨트랙트 이벤트 간의 **지연 시간**을 늘립니다.

다음 보기는 데이터 크기가 어떻게 제한되는지를 보여줍니다:

```
Data - "abcd1234"
Length in string format - 8
Hex Byte representation - [171 205 18 52]
Length in byte format - 4
```

다음 명령 실행:

```bash
cd ~/
git clone https://github.com/maticnetwork/heimdall
cd heimdall

# Checkout to a proper version
# For eg: git checkout v0.2.9-mainnet
git checkout <TAG OR BRANCH>
make install
```

그러면 `heimdalld`와 `heimdallcli` 바이너리가 설치됩니다. 모든 것이 정상인지 확인합니다:

```bash
heimdalld version --long
```

### Bor

***이것은 풀 노드를 위해 필요합니다***

다음으로, 최신 버전의 Bor를 설치합니다. Git을 통해 정식으로 [배포된 버전](https://github.com/maticnetwork/bor/releases)을 체크아웃했는지 확인하십시오.

```bash
cd ~/
git clone https://github.com/maticnetwork/bor
cd bor

# Checkout to a proper version

# For eg: git checkout 0.2.16

git checkout <TAG OR BRANCH>
make bor-all
sudo ln -nfs ~/bor/build/bin/bor /usr/bin/bor
sudo ln -nfs ~/bor/build/bin/bootnode /usr/bin/bootnode
```

그러면 `bor` 바이너리 및 `bootnode` 바이너리가 설치됩니다:

```bash
bor version
```

## 노드 파일 구성하기

### launch 리포지토리 가져오기

```bash
cd ~/
git clone https://github.com/maticnetwork/launch
```

### launch 디렉터리 구성하기

네트워크 디렉토리를 설정하려면, 네트워크 이름과 노드 유형이 필요합니다.

사용가능한 네트워크: `mainnet-v1`

노드 유형: `sentry` 와 `validator`

```bash
cd ~/
mkdir -p node
cp -rf launch/<network-name>/sentry/<node-type>/* ~/node

# To setup sentry node for Polygon mainnet
# cp -rf launch/mainnet-v1/sentry/sentry/* ~/node
```

### 네트워크 디렉토리 구성하기

**Heimdall 데이터 설정**

```bash
cd ~/node/heimdall
bash setup.sh
```

**Bor 데이터 설정**

```bash
cd ~/node/bor
bash setup.sh
```

## 서비스 파일 설정하기

service.sh 파일 다운로드

```bash
cd ~/node
wget https://raw.githubusercontent.com/maticnetwork/launch/master/<network-name>/service.sh
# To setup sentry node for mainnet (mainnet-v1)
# wget https://raw.githubusercontent.com/maticnetwork/launch/master/mainnet-v1/service.sh
```

metadata 파일 생성:
```bash
sudo mkdir -p /etc/matic
sudo chmod -R 777 /etc/matic/
touch /etc/matic/metadata
```

서비스 파일들을 생성하고 system 디렉토리로 복사

```bash
cd ~/node
bash service.sh
sudo cp *.service /etc/systemd/system/
```



## config 파일 설정하기

- 원격 장비/VM에 로그인합니다
- `config.toml` 파을에 몇 가지 세부 정보를 추가해야 합니다. `config.toml` 파일을 열기 위해서 다음 명령을 실행하십시오: `vi ~/.heimdalld/config/config.toml`

    이제 config파일에서 `Moniker`를 변경하고 `seeds` 정보를 추가해야 합니다.

    ```jsx
    moniker=<enter unique identifier> For example, moniker=my-sentry-node
    ```

    ```jsx
    seeds="f4f605d60b8ffaaf15240564e58a81103510631c@159.203.9.164:26656,4fb1bc820088764a564d4f66bba1963d47d82329@44.232.55.71:26656"
    ```

    - **Pex**의 값을 `true`로 변경
    - **Prometheus**의 값을 `true`로 변경
    - `max_open_connections` 값을 `100`으로 설정

   상기와 같이 변경할 때, 올바른 형식을 유지해야 합니다.

- 다음으로 Bor를 위한 `start.sh` 파일을 변경해야 합니다. `vi ~/node/bor/start.sh`의 다음 플래그를 `bor` 시작 매개변수에 추가합니다:

```bash
--bootnodes "enode://0cb82b395094ee4a2915e9714894627de9ed8498fb881cec6db7c65e8b9a5bd7f2f25cc84e71e89d0947e51c76e85d0847de848c7782b13c0255247a6758178c@44.232.55.71:30303,enode://88116f4295f5a31538ae409e4d44ad40d22e44ee9342869e7d68bdec55b0f83c1530355ce8b41fbec0928a7d75a5745d528450d30aec92066ab6ba1ee351d710@159.203.9.164:30303"
```

Archive모드를 활성화하려면 `start.sh` 파일에 다음 플래그를 추가할 수 있습니다

```jsx
--gcmode 'archive' \
--ws --ws.port 8546 --ws.addr 0.0.0.0 --ws.origins '*' \
```

## 서비스 시작하기

Sentry노드에서 다음 명령을 실행하십시오:

**Heimdall 서비스를 시작하기위해**

```jsx
sudo service heimdalld start
```

**Heimdall Rest-server를 시작하기 위해**

```jsx
sudo service heimdalld-rest-server start
```

여기에서 Heimdall과 rest-server에 대한 로그를 확인합니다:

- Heimdall - `journalctl -u heimdalld.service -f`
- Heimdall Rest Server - `journalctl -u heimdalld-rest-server.service -f`

이제 **Heimdall이 완전히 동기화되었는지** 확인한 다음 Bor를 시작해야 합니다. Heimdall을 완전히 동기화하지 않고 Bor를 시작하면 자주 문제가 발생합니다.

- •	Heimdall이 동기화되었는지 확인하려면 
    - 원격 장비/VM에서, `curl localhost:26657/status`를 실행합니다.
    - 출력에서, `catching_up`값은 `false`이어야 합니다.

이제 Heimdall이 동기화되면 다음을 실행하십시오.

```jsx
sudo service bor start
```

여기에서 Bor 로그를 확인할 수 있습니다:

- Bor - `journalctl -u bor.service -f`




</TabItem>

</Tabs>
