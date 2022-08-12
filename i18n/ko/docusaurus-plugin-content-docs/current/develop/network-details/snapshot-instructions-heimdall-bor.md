---
id: snapshot-instructions-heimdall-bor
title: Heimdall과 Bor 스냅샷
description: Heimdall 및 Bor에 대한 스냅샷 지침
keywords:
  - docs
  - matic
  - 폴리곤
  - 바이너리
  - 노드
  - validator
  - sentry
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

새로운 센트리, validator, 또는 풀 노드 서버를 설정할 때, 네트워크를 통해 동기화할 필요없이 더 빠른 동기화를 위해 스냅샷을 활용하는 것이 좋습니다. 스냅샷을 활용하면 Heimdall과 Bor 모두에서 며칠을 절약할 수 있습니다.

:::참고 최신 스냅샷을 보려면 [여기](https://snapshots.matic.today)를 방문하십시오. :::

## Heimdall 스냅샷

먼저 노드 설정 가이드에 따라 **전제 조건**으로 노드를 설정해야 합니다. Heimdall이 동기화할 서비스를 시작하기 전에 아래 단계에 따라 스냅샷을 사용하십시오:

1. 다음 명령을 실행하여 VM에서 Heimdall의 스냅샷 tar 파일을 다운로드 합니다:

```
wget -c <snapshot url>

// For example, this will download the snapshot of Heimdall:
wget -c https://matic-blockchain-snapshots.s3-accelerate.amazonaws.com/matic-mainnet/heimdall-snapshot-2021-09-12.tar.gz
```

2. Heimdall Data 디렉터리에서 tar파일의 압축을 풀려면 다음 명령을 실행합니다:
```
// You must ensure you are running this command
// before you start the Heimdall service on your node.
// If your Heimdall service has started, please stop the service and run the following command:
// Once unpacking is complete, you can start the Heimdall service again:
tar -xzvf <snapshot file> -C <HEIMDALL_DATA_DIRECTORY>

// If your Heimdall data directory is different,
// please replace the directory name in the command for starting the Heimdall service.
// When this command completes, you may delete the tar file to reclaim space.

// For example, this will unpack the tar file in the Heimdall Data directory:
tar -xzvf heimdall-snapshot-2021-09-12.tar.gz -C ~/.heimdalld/data/
```

## Bor 스냅샷

먼저 노드 설정 가이드에 따라 **전제 조건**으로 노드를 설정해야 합니다. Bor가 동기화할 서비스를 시작하기 전에 아래 단계에 따라 스냅샷을 사용하십시오:

1. 다음 명령을 실행하여 VM에서 Bor의 스냅샷 tar 파일을 다운로드합니다:
```
wget -c <snapshot url>

// For example:
wget -c https://matic-blockchain-snapshots.s3-accelerate.amazonaws.com/matic-mainnet/bor-pruned-snapshot-2021-09-08.tar.gz
```
2. Bor Data 디렉터리에서 tar파일의 압축을 풀려면 다음 명령을 실행합니다:

```
// You must ensure you are running this command
// before you start the Bor service on your node.
// If your Bor service has started, please stop the service and run the following command:
// Once unpacking is complete, you can start the Bor service again.

tar -xzvf <snapshot file> -C <BOR_DATA_DIRECTORY>

// If your bor data directory is different
// please replace the directory name in the command for starting the Bor service.
// When this command completes, you may delete the tar file to reclaim space.

// For example, this will unpack the tar file in the Bor data directory:
tar -xzvf bor-pruned-snapshot-2021-09-08.tar.gz -C ~/.bor/data/bor/chaindata
```
