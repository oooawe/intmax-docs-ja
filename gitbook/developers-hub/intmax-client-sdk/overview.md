---
icon: book-open-reader
description: INTMAX Client SDK の概要とアーキテクチャ、基本コンセプト
---

# 概要

INTMAX Client SDK は、INTMAX ネットワークと連携するための安全で高性能なインターフェースを提供します。WebAssembly による最適化された暗号処理を基盤とし、複雑なブロックチェーン操作を開発者フレンドリーな API に抽象化しています。トランザクションの作成・署名からブロードキャストまで、完全なライフサイクルを扱います。
このドキュメントでは、SDK のコア機能、利用可能な関数、インターフェース仕様について説明します。安全なブロックチェーンウォレット統合の構築にお役立てください。

**最新バージョン**: `1.5.2`

**INTMAX Client SDK を GitHub で見る**

[GitHub で見る](https://github.com/InternetMaximalism/intmax2-client-sdk)

**INTMAX Client SDK の使用例**

[GitHub で使用例を見る](https://github.com/InternetMaximalism/intmax2-client-sdk/tree/main/examples)

[GitHub で E2E テスト例を見る](https://github.com/InternetMaximalism/intmax2-e2e)

**INTMAX Client SDK 統合ガイド**

[統合ガイドを開く](developers-hub/intmax-client-sdk/integration-guide.md)

## アーキテクチャ

このセクションでは、INTMAX ネットワークがサポートするコアワークフローである **Deposit**、**Transfer**、**Withdrawal** について説明します。各ワークフローは、セキュリティ、スケーラビリティ、そして Ethereum メインネットと INTMAX ネットワーク間のシームレスな連携を実現するよう設計されています。

### Deposit ワークフロー

Ethereum メインネットから INTMAX ネットワークへ資産を移動します。Predicate コントラクトが制裁対象アドレスからの Deposit をブロックし、不正資金の流入を防止します。その後、Deposit データは Scroll Messenger を通じて Scroll ネットワークに中継され、プロセス全体を通じて最大限のセキュリティが維持されます。

```bash
Ethereum Mainnet
    │
    ▼
[Deposit Contract (Predicate AML)]
    │
    ▼
[Deposit Analyzer]
    │
    ▼
[L1 Scroll Messenger]
    │
    ▼
[Rollup Contract]
    │   └─ A new deposit block is submitted and verified by the block builder
    ▼
INTMAX Network
```

### Transfer ワークフロー

ユーザーが署名したトランザクションは、単一の Transfer としてパッケージ化され、Block Builder によってロールアップコントラクト（Rollup Contract）に送信されます。

INTMAX ネットワーク内での即時かつ低コストな Value Transfer を実現します。

```bash
INTMAX Network
    │
    ▼
[Transfer]
    │
    ▼
[Block Builder]
    │
    ▼
[Rollup Contract]
    │   └─ A new transfer block is submitted and verified by the block builder
    ▼
INTMAX Network
```

### Withdrawal ワークフロー

セキュリティを損なうことなく、INTMAX ネットワークから Ethereum メインネットへ資産を返却します。
Withdrawal リクエストは Withdrawal Aggregator によってバッチ処理され、L2 上で証明されます。その証明は L2 Scroll Messenger を通じて送信され、Withdrawal Messenger Relayer が Ethereum 上に提出します。ERC-20 などの非 ETH トークンの場合、Liquidity Contract が資金のリリース前に追加の Claim ステップを実行し、最小限のガスオーバーヘッドでスムーズなクロスチェーン出金を実現します。

```bash
INTMAX Network
    │
    ▼
[Withdrawal]
    │
    ▼
[Withdrawal Aggregator]
    │
    ▼
[L2 Scroll Messenger]
    │
    ▼
[Withdrawal Messenger Relayer]
    │
    ▼
[Liquidity Contract]
    │   └─ For non-ETH tokens, an additional claim process is required
    ▼
Ethereum Mainnet
```

## 基本コンセプト

### アカウントシステム {#account-system}

INTMAX は Ethereum とは異なる独自のアドレス形式を使用します。

- **アドレスの導出**

  INTMAX アドレスは、指定された Ethereum アドレスから決定論的に導出されますが、INTMAX 独自のエンコーディングスキームを使用するため、結果の値は異なります。

- **パラメータの使い分け**

  | **操作** | **指定するアドレス** |
  | ------------------------------------- | --------------------- |
  | Deposit | INTMAX アドレス |
  | オンチェーントランザクション（INTMAX ネットワーク） | INTMAX アドレス |
  | Withdraw | Ethereum アドレス |

- **環境スコープ**

  導出は環境固有です。同じ Ethereum アドレスから出発しても、メインネットとテストネットで生成される INTMAX アドレスは異なります。トランザクションを送信する前に、どちらのネットワークを対象としているか必ず確認してください。

  **メインネット**: **i**（小文字）で始まる 95 文字の文字列
  - 例: `i9bX5qzARYR7geR35g4K9972DB8fcWqPjNNgQnoGFViZaTLaSiKUTEd7geR35g4K9972DB8fcWqPjNNgQnoGFViZPctJYmE`

  **テストネット**: **T**（大文字）で始まる 95 文字の文字列
  - 例: `T6ubiG36LmNce6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcFtvXnBB3bqice6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcB3prnCZ`

  **⚠️ 重要:** メインネットとテストネットのアドレスを混同しないよう注意してください。実行する操作に必要なアドレスの種類を常に確認し、INTMAX アドレスの導出・使用時には正しい環境（メインネットまたはテストネット）を使用してください。

### 環境設定

INTMAX は **メインネット** と **テストネット** の 2 つの環境で動作します。
同じ Ethereum アドレスから導出しても、INTMAX アドレスは環境ごとに異なります。
SDK の初期化時に環境を明示的に指定してください。

```ts
IntMaxClient.init({ environment: "mainnet" });
IntMaxClient.init({ environment: "testnet" });
```

### プライバシーモデル

INTMAX のプライバシーモデルは、**秘密鍵（Private Key）を持つウォレットオーナーのみが残高とトランザクション履歴を閲覧できる** ことを保証します。

- パブリックブロックチェーンとは異なり、第三者がアドレスの残高や履歴を直接確認することはできません。
- Transfer データは `salt` と `nullifier` の値を使用して受取人のアドレスを隠し、トランザクション同士の紐付けを防止します。

### バッチサイズ制限

SDK は最適なパフォーマンスのためにバッチサイズ制限を設けています：

- **トランザクションのブロードキャスト**: `broadcastTransaction` 呼び出しあたり最大 63 トランザクション
- **データ取得**: 内部 API リクエストあたり最大 64 アイテム。SDK は `fetchDeposits()`、`fetchTransfers()`、`fetchTransactions()` などの高レベル API に対してページネーションを自動的に処理します。

詳細は [API リファレンス](developers-hub/intmax-client-sdk/api-reference.md#batch-size-limits) を参照してください。

## Tips

### ローカル Balance Prover の実行方法

ローカルに Balance Prover インスタンスをセットアップし、リクエストを送信できます。

#### 1. リポジトリのクローン

GitHub から `intmax2` リポジトリ（`main` ブランチ）をローカル環境にクローンします。

```bash
git clone git@github.com:InternetMaximalism/intmax2.git -b main
```

#### 2. Balance Prover ディレクトリへ移動

クローンしたリポジトリ内の `balance-prover` ディレクトリに移動します。

```bash
cd intmax2/balance-prover
```

#### 3. 環境設定ファイルの準備

提供されている `.example.env` テンプレートを基に、環境設定ファイル `.env` を作成します。

```bash
cp -n .example.env .env
```

#### 4. Balance Prover の起動

Cargo を使用してリリースモード（`-r`）で Balance Prover を実行します。

```bash
cargo run -r
```

#### 5. SDK 設定の変更

ブラウザと Node.js それぞれの設定更新方法を示します。

**5-a. ブラウザ**

`http://localhost:9001` でホストされるプライベート ZKP サーバーを使用するには、以下のコードを使用します：

```tsx
import { IntMaxClient } from "intmax2-client-sdk";

const intMaxClient = IntMaxClient.init({
  environment: "mainnet",
  urls: {
    balance_prover_url: "http://localhost:9001",
    use_private_zkp_server: false,
  },
});
```

ローカルで Balance Prover を実行する場合は、`use_private_zkp_server` を `false` に設定してください。

**5-b. Node.js**

`http://localhost:9001` でホストされるプライベート ZKP サーバーを使用するには、以下のコードを使用します：

```tsx
const intMaxClient = new IntMaxNodeClient({
  environment: "mainnet",
  eth_private_key: process.env.ETH_PRIVATE_KEY,
  l1_rpc_url: process.env.L1_RPC_URL,
  urls: {
    balance_prover_url: "http://localhost:9001",
    use_private_zkp_server: false,
  },
});
```

ローカルで Balance Prover を実行する場合は、`use_private_zkp_server` を `false` に設定してください。

{% hint style="info" icon="dna" %}
We welcome contributions and feedback!

Please feel free to open an [issue](https://github.com/InternetMaximalism/intmax2-client-sdk/issues) or submit a [pull request](https://github.com/InternetMaximalism/intmax2-client-sdk/pulls) on GitHub.

Your support helps improve the INTMAX ecosystem.
{% endhint %}

## リファレンス

以下に Rust コードとコントラクトのドキュメントへのリンクを掲載します。参考資料としてご利用ください。

### スマートコントラクト

デプロイ済みコントラクトとその操作方法に関するドキュメントです。

[**スマートコントラクトのドキュメントを見る**](developers-hub/intmax-nodes/smart-contracts.md)

### Rust CLI

Rust を使用して INTMAX ネットワークと連携するためのコマンドラインインターフェースです。

[**GitHub で INTMAX CLI を見る**](https://github.com/InternetMaximalism/intmax2/tree/main/cli)

### Rust SDK

INTMAX ネットワークと外部から連携するための公式 Rust SDK です。

[**GitHub で INTMAX Client SDK を見る**](https://github.com/InternetMaximalism/intmax2/tree/main/client-sdk)

> この SDK により、Rust を使用して INTMAX ネットワークとシームレスに連携できます。
