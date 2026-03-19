---
icon: cubes
description: Full Network モードでの Block Builder セットアップと運用手順
---

# Full Network

Block Builder ノードは、INTMAX Network にブロックを送信する重要なコンポーネントです。

このガイドでは、自動セットアップスクリプトを使用して **Full Network モード**で INTMAX2 Block Builder をセットアップ・実行する手順を説明します。より柔軟なサーバーサイド管理や、複数の Block Builder を異なる環境にデプロイしたい場合は、**Standalone モード**のドキュメントを参照してください。

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

#### ⚠️ セキュリティに関する重要な注意

> **インターネットからスクリプトをダウンロードして実行する際は十分ご注意ください。** 実行前に必ずスクリプトのソースと内容を確認してください。ソースを信頼でき、スクリプトの動作を理解した上で実行してください。

> **必ず以下に記載された公式 INTMAX2 リポジトリの URL からダウンロードしてください。** これにより、正規のスクリプトを入手できます。

### 前提条件

- Docker がシステムにインストールされていること
  - **[Windows Docker リンク](https://docs.docker.com/desktop/install/windows-install)**
  - **[Mac Docker リンク](https://docs.docker.com/desktop/install/mac-install)**
  - **[Linux Docker リンク](https://docs.docker.com/desktop/install/linux-install)**

- Block Builder 用の有効な秘密鍵と、Scroll Sepolia テストネットに最低 **0.01 ETH**
- **L2 RPC URL** へのアクセス
- bash シェルが使える Linux/Windows/macOS 環境

### クイックセットアップ

#### 1. セットアップスクリプトのダウンロード

⚠️ **重要:** セキュリティと信頼性を確保するため、必ず公式 [**INTMAX2 GitHub リポジトリ**](https://github.com/InternetMaximalism/intmax2) からダウンロードしてください。

```bash
# Mainnet
curl -o builder.sh https://raw.githubusercontent.com/InternetMaximalism/intmax2/refs/heads/main/scripts/block-builder-mainnet.sh
chmod +x builder.sh

# Testnet
curl -o builder.sh https://raw.githubusercontent.com/InternetMaximalism/intmax2/refs/heads/main/scripts/block-builder-testnet.sh
chmod +x builder.sh
```

#### 2. 設定の初期化

```bash
# Generate configuration files
./builder.sh setup
```

このコマンドにより、以下のファイルが作成されます。

- `frpc.toml` — FRP クライアント設定
- `nginx.conf` — Nginx プロキシ設定
- `docker-compose.yml` — Docker サービス設定
- `.env` — 環境変数

#### 3. 環境変数の設定

**重要なステップ**: L2 RPC URL と秘密鍵の両方を、統合環境セットアップコマンドで設定します。

```bash
# Configure environment variables interactively
./builder.sh setup-env
```

この対話型コマンドでは、以下の入力を求められます。

1. **L2 RPC URL**: Scroll Sepolia の RPC エンドポイント

   **Scroll Mainnet RPC の例**
   - `https://rpc.ankr.com/scroll`
   - `https://scroll-mainnet.infura.io/v3/YOUR_PROJECT_ID`
   - `https://scroll-mainnet.g.alchemy.com/v2/YOUR_API_KEY`

   **Scroll Testnet RPC の例**
   - `https://rpc.ankr.com/scroll_sepolia_testnet`
   - `https://scroll-sepolia.infura.io/v3/YOUR_PROJECT_ID`
   - `https://scroll-sepolia.g.alchemy.com/v2/YOUR_API_KEY`

2. **秘密鍵**: ウォレットの秘密鍵（`0x` プレフィックスの有無は問いません）
   - 関連するウォレットに **Scroll Sepolia テストネットで最低 0.01 ETH** があることを確認してください
   - 秘密鍵は Docker シークレットとして安全に保存されます

#### 4. 設定の検証（オプション）

```bash
# Verify all configurations
./builder.sh verify-env
```

このコマンドでは以下を検証します。

- L2 RPC URL のフォーマットとアクセス可能性
- 秘密鍵のフォーマットと Docker シークレットの保存状態
- 設定の概要表示

#### 5. 最終チェック

```bash
# Comprehensive configuration check
./builder.sh check
```

以下が表示されます。

- すべての必須ファイルのステータス
- Docker のインストール状態
- 環境変数の検証結果
- Block Builder のドメイン URL

#### 6. Block Builder の起動

```bash
# Start all services as Docker Stack
./builder.sh run
```

#### 7. ヘルスチェック

```bash
# Health check your block builder
./builder.sh health
```

#### 8. サービスの監視

```bash
# Monitor running services and view logs
./builder.sh monitor
```

### 利用可能なコマンド

| コマンド | 説明 |
| --------------------------- | ---------------------------------------------------------------- |
| `./builder.sh setup`        | 一意の UUID を使用して設定ファイルを作成 |
| `./builder.sh setup-env`    | 対話型の環境設定（L2_RPC_URL + 秘密鍵） |
| `./builder.sh verify-env`   | 環境設定の検証 |
| `./builder.sh check`        | 設定の検証と現在の設定内容の表示 |
| `./builder.sh run`          | すべてのサービスを Docker Stack で起動 |
| `./builder.sh stop`         | すべての Docker Stack サービスを停止 |
| `./builder.sh health`       | サービスのヘルスチェック |
| `./builder.sh monitor`      | サービスステータスの監視とログの表示 |
| `./builder.sh update`       | スクリプトを最新バージョンに更新 |
| `./builder.sh clean`        | すべての設定ファイルと Docker リソースを削除 |
| `./builder.sh docker-clean` | Docker コンテナ、イメージ、ネットワークを削除 |
| `./builder.sh version`      | バージョン情報の表示 |

### ヘルスチェック

#### Block Builder が正しく動作しているか確認するには？

このセクションでは、Block Builder インスタンスが正しく動作しているかを検証する方法と、ステータスや手数料情報を取得する方法を説明します。

Block Builder エンドポイントの例：

- `https://proxy.builder.intmax.io/b7d899c2-6a77-4f7e-9c00-d970e9d6fb48`

**ヘルスチェックと手数料情報**

Block Builder が正しく動作しているか確認します。

```bash
./builder.sh health
```

## FAQ

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
        "address": "0xe5b4920e15587582e40c645c98b5d6c539ddb84f",
        "url": "https://stage.api.node.intmax.io/block-builder"
    },
    {
        "address": "0x9175f73999dacbdb1fd7b3cdafed331d29d1ed0b",
        "url": "https://stage.api.node.intmax.io/secondary-block-builder"
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

### Q: 複数の Block Builder を運用したい場合はどうすればよいですか？

複数の Block Builder を運用したい場合（スケーリングや冗長性のためなど）は、Standalone Block Builder ドキュメントに記載されている **Standalone モードのセットアップ**を使用してください。

Standalone モードでは：

- 各 Block Builder は独立して動作し、異なるクラウド環境にデプロイできます
- 各 Block Builder が Indexer からアクセス可能になるよう、ネットワークアクセス（ファイアウォールルール、ポート、リバースプロキシなど）の設定はご自身で行う必要があります
- Indexer は**パブリックにアクセス可能な** Block Builder の URL のみを登録します

このモードは複数の Builder を並行運用するのに適しており、それぞれ異なる URL で登録できます。

👉 セットアップ手順は [Block Builder セットアップ: Standalone](./standalone.md) を参照してください。

## トラブルシューティング

### よくある問題

1. **プレースホルダー値が更新されていない**

   ```
   ⚠️ WARNING: BLOCK_BUILDER_PRIVATE_KEY is still set to placeholder value
   ```

   対処法: `.env` ファイルを編集し、プレースホルダー値を実際の値に置き換えてください

2. **Docker が見つからない**

   ```
   ❌ Docker not found
   ```

   対処法: 公式インストールガイドに従って Docker をインストールしてください

3. **設定ファイルが見つからない**

   ```
   ❌ Configuration files not found
   ```

   対処法: `./builder.sh setup` を実行して設定ファイルを生成してください

4. **Docker Swarm が有効でない**

   以下を実行した場合：

   ```bash
   ./builder.sh setup-env
   ```

   次のエラーが表示されることがあります。

   ```
   ❌ Docker Swarm is not active
   💡 Run: docker swarm init
   ```

   これは**マシン上で Docker Swarm モードが初期化されていない**ことを意味します。
   - Docker Swarm は `docker stack deploy` の使用やスタック内で定義されたサービスの管理に必要です。

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
