---
icon: cubes
description: Standalone モードでの Block Builder セットアップと運用手順
---

# Standalone

Block Builder ノードは、INTMAX Network にブロックを送信する重要なコンポーネントです。

このドキュメントでは、**Standalone モード**で Block Builder を効率的にセットアップ・運用するための詳細な手順を説明します。このモードでは、インターネットから Block Builder へのネットワークアクセス設定（ファイアウォール、ポート、ロードバランサーなど）をご自身で行う必要があります。Standalone でありながら、クラウド環境で複数の Block Builder インスタンスをデプロイでき、スケーラブルかつ冗長な構成が可能です。

**Standalone モードはより高い柔軟性とカスタマイズ性を提供**し、オペレーターがインフラや運用ニーズに応じたセットアップを構築できます。

**注意**: **Mainnet** と **Testnet** の2つのバージョンを提供しています。混同しないようご注意ください。

## 主な特徴

Block Builder は、秘密鍵（Private Key）で指定された Ethereum アカウントから **Scroll** にブロックを送信します。Scroll ネットワークとやり取りするコアコンポーネントであり、将来の機能拡張に対応するため、外部トランザクションを受け付ける設計になっています。

メインネット環境で安定した運用を行うため、Block Builder は継続的に稼働させる必要があります。

- **トランザクションの送信：**
  - Block Builder は外部ソースからトランザクションを収集し、ブロックとして Scroll ネットワークに送信します。これにより、ネットワーク全体との効率的なやり取りとシームレスなブロック送信を実現します。
- **手数料の収集：**
  - Block Builder はトランザクションごとにユーザーあたり $0.005 の手数料を収集します。
  - 1ブロックあたり最大 $0.005 × 128 の手数料を蓄積でき、スケーラブルでインセンティブのある運用を支えます。
- **オンラインステータスの通知：**
  - Block Builder は1日1回、[**Block Builder Registry Contract**](/developers-hub/intmax-nodes/smart-contracts#block-builder-registry) にトランザクションを送信して、[**Indexer**](/developers-hub/intmax-nodes/indexer) にオンラインであることを通知します。
  - Indexer は最適な Block Builder の URL をユーザーに配信する重要な役割を担っており、効率的で信頼性の高いネットワークインタラクションを確保します。

## 要件

- **推奨構成：**
  - 2 vCPU
  - 4 GB RAM
  - 10 GB SSD
- **最小構成：**
  - 1 vCPU
  - 1 GB RAM
  - 4 GB SSD

## はじめに

### 環境変数の設定

`.env` ファイルを作成し、以下の設定を行います。

プレースホルダーを実際の認証情報と URL に置き換えてください。

- `BLOCK_BUILDER_PRIVATE_KEY=<private-key>`: Scroll Network 上で ETH を保有する Ethereum アカウントの秘密鍵
- `BLOCK_BUILDER_URL=<your-block-builder-url>`: **インターネットからアクセス可能な** Block Builder の URL。ポート番号がある場合は必ず含めてください

  ⚠️ **注意:** 安全な通信を確保するため、`http://` ではなく必ず `https://` を使用してください。

- `L2_RPC_URL=<your-scroll-rpc-url>`: Scroll Network の RPC エンドポイント。Alchemy ダッシュボードで Scroll ネットワークが有効になっていることを確認してください

**重要:** 秘密鍵（`<private-key>`）は絶対に公開・共有しないでください。アカウントへの不正アクセスを防ぐため、安全に管理してください。安定した運用のため、**Block Builder は Scroll Network 上で常に最低 0.1 ETH の残高を維持することを推奨します**。

**注意:** **テストネット**環境を使用する場合は、RPC URL を Scroll Sepolia の RPC エンドポイントに置き換え、Scroll Sepolia Network 上で ETH を保有するアカウントを使用してください。また、Alchemy ダッシュボードで Scroll ネットワークが有効になっていることを確認してください。

`.env` ファイルで値を指定する際は、**引用符を使用しないでください**。

```bash
# Setup .env
vim .env
```

#### [Mainnet]

```bash
#######
# Contents of .env for mainnet
#######

# app settings
PORT=8080
BLOCK_BUILDER_PRIVATE_KEY=<private-key>

# builder settings
ETH_ALLOWANCE_FOR_BLOCK=0.001
TX_TIMEOUT=80
ACCEPTING_TX_INTERVAL=30
PROPOSING_BLOCK_INTERVAL=30
INITIAL_HEART_BEAT_DELAY=180
HEART_BEAT_INTERVAL=85800
GAS_LIMIT_FOR_BLOCK_POST=400000
CLUSTER_ID=1
BLOCK_BUILDER_URL=<your-block-builder-url>

# fee settings
REGISTRATION_FEE=0:2500000000000
NON_REGISTRATION_FEE=0:2000000000000

# external settings
ENV=prod
STORE_VAULT_SERVER_BASE_URL=https://api.node.intmax.io/store-vault-server
USE_S3=true
VALIDITY_PROVER_BASE_URL=https://api.node.intmax.io/validity-prover
L2_RPC_URL=<your-scroll-rpc-url>
ROLLUP_CONTRACT_ADDRESS=0x1c88459D014e571c332BF9199aD2D35C93219A2e
BLOCK_BUILDER_REGISTRY_CONTRACT_ADDRESS=0x79dA6F756D26c50bA74bF9634bd3543645058b5B
```

#### [Testnet β]

```bash
#######
# Contents of .env for testnet
#######

# app settings
PORT=8080
BLOCK_BUILDER_PRIVATE_KEY=<private-key>

# builder settings
ETH_ALLOWANCE_FOR_BLOCK=0.001
TX_TIMEOUT=80
ACCEPTING_TX_INTERVAL=30
PROPOSING_BLOCK_INTERVAL=30
INITIAL_HEART_BEAT_DELAY=180
HEART_BEAT_INTERVAL=85800
GAS_LIMIT_FOR_BLOCK_POST=400000
CLUSTER_ID=1
BLOCK_BUILDER_URL=<your-block-builder-url>

# fee settings
REGISTRATION_FEE=0:2500000000000
NON_REGISTRATION_FEE=0:2000000000000

# external settings
ENV=staging
STORE_VAULT_SERVER_BASE_URL=https://stage.api.node.intmax.io/store-vault-server
USE_S3=true
VALIDITY_PROVER_BASE_URL=https://stage.api.node.intmax.io/validity-prover
L2_RPC_URL=<your-scroll-sepolia-rpc-url>
ROLLUP_CONTRACT_ADDRESS=0xcEC03800074d0ac0854bF1f34153cc4c8bAEeB1E
BLOCK_BUILDER_REGISTRY_CONTRACT_ADDRESS=0x93a41F47ed161AB2bc58801F07055f2f05dfc74E
```

### Docker での実行（Linux）

以下のコマンドを実行する前に、**`<release-version>`** を公式リリースページで公開されている最新バージョンに置き換えてください。

**注意:** リリースバージョンに `v` プレフィックスは付けないでください。

👉 **最新リリースは[こちら](https://github.com/InternetMaximalism/intmax2/pkgs/container/intmax2)で確認**

コマンドを実行する前に、`.env` ファイルのセットアップまたは必要な環境変数の設定が完了していることを確認してください。

⚡ ヒント: ログが不要でサーバーのストレージ使用量を抑えたい場合は、Docker を `--log-driver=none` オプションで実行できます。これによりログの永続化が無効になり、ディスクの圧迫を防げます。

**x86-64 アーキテクチャ**

```bash
docker pull ghcr.io/internetmaximalism/intmax2:**<release-version>**
docker run -d --rm \
    --env-file ./.env \
    --name block-builder \
    --log-driver=none \
    -p 8080:8080 \
     ghcr.io/internetmaximalism/intmax2:**<release-version>** /app/block-builder

##########
# example
##########
docker pull ghcr.io/internetmaximalism/intmax2:0.1.34
docker run -d --rm \
    --env-file ./.env \
    --name block-builder \
    --log-driver=none \
    -p 8080:8080 \
     ghcr.io/internetmaximalism/intmax2:0.1.34 /app/block-builder
```

**arm-64 アーキテクチャ**

```bash
docker pull ghcr.io/internetmaximalism/intmax2:**<release-version>**-arm64
docker run -d --rm \
    --env-file ./.env \
    --name block-builder \
    --log-driver=none \
    -p 8080:8080 \
     ghcr.io/internetmaximalism/intmax2:**<release-version>**-arm64 /app/block-builder

##########
# example
##########
docker pull ghcr.io/internetmaximalism/intmax2:0.1.34-arm64
docker run -d --rm \
    --env-file ./.env \
    --name block-builder \
    --log-driver=none \
    -p 8080:8080 \
     ghcr.io/internetmaximalism/intmax2:0.1.34-arm64 /app/block-builder
```

### バイナリでの実行（Linux）

以下のコマンドを実行する前に、**`<release-version>`** を公式リリースページで公開されている最新バージョンに置き換えてください。

**注意:** リリースバージョンには `v` プレフィックスを付けてください。

👉 **最新リリースは[こちら](https://github.com/InternetMaximalism/intmax2/releases)で確認**

コマンドを実行する前に、`.env` ファイルのセットアップまたは必要な環境変数の設定が完了していることを確認してください。

**x86-64 アーキテクチャ**

```bash
curl -L https://github.com/InternetMaximalism/intmax2/releases/download/**<release-version>**/intmax2-x86_64-unknown-linux-gnu.tar.gz \
| tar -xzv
chmod +x block-builder
./block-builder

##########
# example
##########
curl -L https://github.com/InternetMaximalism/intmax2/releases/download/v0.1.34/intmax2-x86_64-unknown-linux-gnu.tar.gz \
| tar -xzv
chmod +x block-builder
./block-builder
```

**arm-64 アーキテクチャ**

```bash
curl -L https://github.com/InternetMaximalism/intmax2/releases/download/<release-version>/intmax2-aarch64-unknown-linux-gnu.tar.gz \
| tar -xzv
chmod +x block-builder
./block-builder

##########
# example
##########
curl -L https://github.com/InternetMaximalism/intmax2/releases/download/v0.1.34/intmax2-aarch64-unknown-linux-gnu.tar.gz \
| tar -xzv
chmod +x block-builder
./block-builder
```

## ヘルスチェック

### Q: Block Builder が正しく動作しているか確認するには？

これらのエンドポイントは、Block Builder のヘルス、動作状況、手数料の詳細に関する重要な情報を提供します。ローカルで動作を確認した後、**エンドポイントが外部ソースからもアクセス可能であることを必ず確認してください。** これにより、ネットワーク参加者があなたの Block Builder と通信できるようになります。ファイアウォール設定やリバースプロキシが、適切なポートへの外部アクセスを許可していることを確認してください。

**ローカルチェック**

まず、Block Builder がローカルで動作していることを確認します。

```bash
curl http://localhost:8080/health-check

# response
{"name":"block-builder","version":"0.1.34"}
```

**外部チェック**

ローカルでの動作確認後、ヘルスチェックエンドポイントが外部ソースからもアクセス可能であることを確認します。`<your-domain>` を実際のドメインまたは IP アドレスに置き換えてください。

```bash
curl https://<your-domain>/health-check

# response
{"name":"block-builder","version":"0.1.34"}
```

> ⚠️ ファイアウォールルール、リバースプロキシ（例: Nginx）、またはクラウドサービスの設定で、エンドポイントへの外部 HTTP/HTTPS アクセスが許可されていることを確認してください。

### Q: Block Builder が登録済みで稼働準備ができているか確認するには？

以下の API を呼び出すことで、Block Builder の登録状況を確認できます。

```bash
# Mainnet
curl https://api.indexer.intmax.io/v1/indexer/builders/registration/<your-block-builder-address>

# Testnet
curl https://stage.api.indexer.intmax.io/v1/indexer/builders/registration/<your-block-builder-address>
```

`<your-block-builder-address>` を実際の Block Builder アドレスに置き換えてください。

レスポンスは以下のようになります。

```json
{
  "ready": true,
  "registered": true
}
```

- `registered`: Block Builder が Indexer に登録済みかどうかを示します
- `ready`: Block Builder が手数料と残高のチェックをパスし、アクティブな Builder リストに含まれているかどうかを示します

この API を使用して、Indexer における Block Builder の現在のステータスを確認できます。

### 手数料情報

#### ローカルチェック

Block Builder の手数料関連データをローカルで確認します。

```bash
curl http://localhost:8080/fee-info

# Expected response
{
    "beneficiary": "i9ewLfwvXpw9LY1dW5NmbZKmTkUdW6U8feffTigEyc5QaTofZnzcA8pCu18tYDA8EX736gkEkU7Tj5CCDGogaBbFQQbc5Wv",
    "blockBuilderAddress": "0x9ac5289697c0fae66a31337c0447bea38bffa5ee",
    "nonRegistrationCollateralFee": null,
    "nonRegistrationFee": [
        {
            "amount": "2000000000000",
            "token_index": 0
        }
    ],
    "registrationCollateralFee": null,
    "registrationFee": [
        {
            "amount": "2500000000000",
            "token_index": 0
        }
    ],
    "version": "0.1.34"
}

```

#### 外部チェック

ローカルでの動作確認後、fee-info エンドポイントが外部ソースからもアクセス可能であることを確認します。`<your-domain>` を実際のドメインまたは IP アドレスに置き換えてください。

```bash
curl https://<your-domain>/fee-info

# Expected response
{
    "beneficiary": "i9ewLfwvXpw9LY1dW5NmbZKmTkUdW6U8feffTigEyc5QaTofZnzcA8pCu18tYDA8EX736gkEkU7Tj5CCDGogaBbFQQbc5Wv",
    "blockBuilderAddress": "0x9ac5289697c0fae66a31337c0447bea38bffa5ee",
    "nonRegistrationCollateralFee": null,
    "nonRegistrationFee": [
        {
            "amount": "2000000000000",
            "token_index": 0
        }
    ],
    "registrationCollateralFee": null,
    "registrationFee": [
        {
            "amount": "2500000000000",
            "token_index": 0
        }
    ],
    "version": "0.1.34"
}

```

## FAQ

### Q: Indexer コンポーネントの概要と Block Builder の要件

**Indexer** は、複数の有効な **Block Builder** の URL を管理し、ユーザーに適切な Block Builder の URL を返すコンポーネントです。

Indexer は、以下のすべての条件を満たす Block Builder のみを返します。

- **定期的に HeartBeat シグナルを送信している**
- **登録された URL がアクセス可能である**
- **残高が最低 0.001 ETH ある**

Block Builder はこれらの条件を満たすように設定する必要があります。

### Q: 自分の Block Builder によるブロック送信の確認方法

ブロック内の minter アドレスが、Block Builder に設定したアドレスと一致していれば、そのブロックはあなたの Block Builder によって送信されたものです。

送信されたブロックは以下の Explorer で確認できます。

- [Mainnet Explorer](https://explorer.intmax.io/)
- [Testnet Explorer](https://beta.testnet.explorer.intmax.io/)

### Q: Docker コンテナ内の同期データはどこに保存されますか？

A: Block Builder はデータを永続的に保存しません。トランザクションデータを受信してブロックとして送信するのみです。トランザクションデータは一時的にメモリに保持されますが、ディスクに書き込まれたりコンテナ内に永続的に保存されたりすることはありません。そのため、同期データが保存される特定のディレクトリはありません。

### Q: Ubuntu で `libssl.so.1.1` ライブラリが見つからないエラーの解決方法

Ubuntu で `libssl.so.1.1` が見つからないエラーが発生した場合、以下の手順で必要なパッケージを手動インストールできます。

```bash
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.23_amd64.deb
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.23_amd64.deb
```

### Q: `L2_RPC_URL` に Alchemy 以外の RPC URL を使用できますか？

A: はい、`L2_RPC_URL` には Alchemy 以外の RPC プロバイダーも使用できます。代替プロバイダーの例：

- **Infura**
- **Ankr**
- **カスタム RPC**: 自前のセルフホストノードの RPC URL

パフォーマンスと安定性は RPC プロバイダーによって異なる場合があるため、環境とユースケースに最適なものを選択してください。

### Q: `BLOCK_BUILDER_URL` には何を設定すればよいですか？

Block Builder にアクセスできる `BLOCK_BUILDER_URL` を設定してください。ユーザーはこの URL を通じてあなたの Block Builder にトランザクションを送信します。URL がアクセスできない場合、Block Builder はブロックを送信できません。

`${BLOCK_BUILDER_URL}/health-check` にアクセスして確認できます。URL がアクセス可能であれば、以下のようなヘルスチェックレスポンスが返されます。

```bash
{ "name":"block-builder","version":"0.1.34" }
```

### Q: メインネットでの Block Builder 1回の送信にかかるガス代はいくらですか？

Block Builder が1ブロックを送信するのに必要なガス代は約 **0.000016 ETH** です。安定した運用のためには、この金額よりも多めに保持しておくことをお勧めします。メインネットや Scroll ネットワークの混雑状況によりガス代は変動する場合があるのでご注意ください。

### Q: Indexer に自分の Block Builder が登録されているか確認する方法

以下の URL にリクエストを送信してください。Indexer に登録されているアドレスのうち3件がランダムに返されます。リクエストを数回繰り返すと、現在アクティブな Block Builder のリストを確認できます。

```bash
# Mainnet
curl https://api.indexer.intmax.io/v1/indexer/builders

[
  {
    "address": "0x9ac5289697c0fae66a31337c0447bea38bffa5ee",
    "url": "https://api.node.intmax.io/block-builder"
  },
  {
    "address": "0xa5de22aef9770067cc5284d94dab623c3cefa049",
    "url": "https://api.node.intmax.io/secondary-block-builder"
  }
]

# Testnet
curl https://stage.api.indexer.intmax.io/v1/indexer/builders

[
    {
        "address": "0x9ac5289697c0fae66a31337c0447bea38bffa5ee",
        "url": "https://api.node.intmax.io/block-builder"
    },
    {
        "address": "0xa5de22aef9770067cc5284d94dab623c3cefa049",
        "url": "https://api.node.intmax.io/secondary-block-builder"
    }
]
```

### Q: Block Builder の URL が Indexer に登録されるタイミングは？

Block Builder の稼働開始後、`INITIAL_HEART_BEAT_DELAY` で設定された時間が経過すると、Block Builder Registry Contract に URL が登録されます。
**URL が有効であれば、その 10〜15 分後に Indexer に登録されます。**
登録されるためには、Indexer 側から URL にアクセスできる必要があります。

### Q: Deposit に必要な最低 ETH 量は？（安定運用にはどれくらいの ETH が必要？）

Block Builder を安定して運用するには、**最低 0.01 ETH を Deposit してください**。

1ブロックの送信に必要なガス代は約 **0.000016 ETH** ですが、ネットワークの混雑状況により変動します。残高が **0.001 ETH** を下回ると、ブロックの送信ができなくなります。

**推奨される残高管理：**

- 目安として、アカウントに**常に 0.01 ETH 以上**を維持してください
- **残高が 0.002 ETH を下回った場合**、できるだけ早くチャージしてください
- **残高が 0.001 ETH を下回った場合**、ブロック送信が失敗するため、定期的な残高確認と適時のチャージを強く推奨します

### Q: Block Builder でカスタム手数料を設定できますか？

はい、Block Builder ではカスタム手数料を設定できます。対応トークンは `ETH`、`~~USDC`、`WBTC`~~ で、それぞれ `tokenIndex` で以下のように識別されます。

- `tokenIndex: 0` → ETH
- ~~`tokenIndex: 1` → ITX~~
- ~~`tokenIndex: 2` → WBTC~~
- ~~`tokenIndex: 3` → USDC~~

たとえば、`0:2500000000000` を指定すると、手数料は 0.0000025 ETH に設定されます。

複数のトークンの手数料を同時に定義することもできます。

例：

`REGISTRATION_FEE=0:2500000000000~~,1:100000~~`

これにより、手数料は 0.0000025 ETH ~~および 1 USDC~~ に設定されます。

## リファレンス

INTMAX2 ネットワークで開発を行う際に役立つリソースです。

### INTMAX2 Block Builder

GitHub でソースコードと実装の詳細を確認できます。

[Block Builder リポジトリを見る](https://github.com/InternetMaximalism/intmax2/tree/main/block-builder)

### INTMAX Mainnet

- App フロントエンド:
  [Mainnet App を開く](https://app.intmax.io/)
- Explorer:
  [Mainnet Explorer を開く](https://explorer.intmax.io/)

### INTMAX Testnet

- App フロントエンド:
  [Testnet App を開く](https://testnet.app.intmax.io/)
- Explorer:
  [Testnet Explorer を開く](https://beta.testnet.explorer.intmax.io/)

### スマートコントラクト

デプロイ済みスマートコントラクトとその使い方に関するドキュメントです。

[スマートコントラクトのドキュメントを見る](../intmax-nodes/smart-contracts.md)
