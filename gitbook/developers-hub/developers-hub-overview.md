---
icon: compass
description: Developers Hub の概要と各セクションへのナビゲーション
---

# Developers Hub 概要

## INTMAX Network の開発者向け総合ガイド
— Welcome to the INTMAX Network Developers Hub —

{% hint style="info" %}
dApp開発、ウォレット構築、インフラのセットアップなど、INTMAX Network 上でアプリケーションを効率的に構築・統合・デプロイするための包括的なリソースとそれらの解説
{% endhint %}

## クイックスタート

### はじめに

- **[Core Concepts](/gitbook/developers-hub/core-concepts/rollup-architecture.md)** — INTMAX のステートレス（Stateless）でプライバシーファーストな L2（Layer 2）アーキテクチャの理解
- **[Payment Lifecycle](/gitbook/developers-hub/payment-lifecycle.md)** — ZK proof を用いた Deposit・Transfer・Withdrawal の流れ
- **[INTMAX Nodes](/gitbook/developers-hub/intmax-nodes/node-reference.md)** — ネットワークを構成する各ノードタイプとその役割

### 開発ツール

- **[INTMAX Client SDK](/gitbook/developers-hub/intmax-client-sdk/overview.md)** — WebAssembly ベースの高パフォーマンス暗号処理 SDK
- **[INTMAX CLI](/gitbook/developers-hub/intmax-cli.md)** — CSV 対応で最大 63 件の Transfer をバッチ処理できるコマンドラインツール
- **[INTMAX Wallet SDK](/gitbook/developers-hub/intmax-wallet-sdk/wallet-sdk-reference.md)** — ウォレット ↔ dApp 統合のためのプロトコルと SDK
- **[コードリポジトリ](/gitbook/developers-hub/code-repository.md)** — INTMAX 各コンポーネントの公式 GitHub リポジトリ

### 統合パス

- **[dApp 開発者向け](/gitbook/#for-dapp-developers)** — INTMAX 上でのアプリケーション構築を開始
- **[ウォレット開発者向け](/gitbook/#for-wallet-developers)** — ウォレットへの INTMAX サポートの統合
- **[インフラプロバイダー向け](/gitbook/#for-infra-providers)** — ネットワークノードのデプロイと運用

## ネットワークインフラ

### コアノード

- **[ノードリファレンス](/gitbook/developers-hub/intmax-nodes/node-reference.md)** — 全ノードタイプの概要とデプロイガイド
- **[Block Builder](/gitbook/developers-hub/intmax-nodes/block-builder.md)** — トランザクションをブロックに集約してリワードを獲得
- **[Validity Prover](/gitbook/developers-hub/intmax-nodes/validity-prover.md)** — Merkle Tree のホストと ZK proof の生成
- **[Withdrawal Server](/gitbook/developers-hub/intmax-nodes/withdrawal-server.md)** — Withdrawal リクエストの処理と L1 との同期
- **[Provers](/gitbook/developers-hub/intmax-nodes/provers.md)** — ネットワーク検証のための暗号プルーフ生成

### ブリッジインフラ

- **[Deposit Relayer](/gitbook/developers-hub/intmax-nodes/deposit-relayer.md)** — Ethereum から INTMAX への資産ブリッジ
- **[Withdrawal Relayer](/gitbook/developers-hub/intmax-nodes/withdrawal-relayer.md)** — Withdrawal のオンチェーン同期
- **[Withdrawal Aggregator](/gitbook/developers-hub/intmax-nodes/withdrawal-aggregator.md)** — L2 → L1 の Withdrawal Claim のバンドル
- **[Claim Aggregator](/gitbook/developers-hub/intmax-nodes/claim-aggregator.md)** — マイニングリワード Claim のバンドルと検証

### サポートサービス

- **[Store Vault Server](/gitbook/developers-hub/intmax-nodes/store-vault-server.md)** — プライベートデータのバックアップとリストア
- **[Indexer](/gitbook/developers-hub/intmax-nodes/indexer.md)** — ノードディスカバリーとデータフィードの提供
- **[スマートコントラクト](/gitbook/developers-hub/intmax-nodes/smart-contracts.md)** — INTMAX の L1 コントラクトスイート

## Use Cases とアプリケーション

### 実用アプリケーション

- **[決済](/gitbook/developers-hub/use-cases/payments.md)** — 高速かつ低コストなトークン Transfer
- **[マイクロトランザクションとキャッシュバック](/gitbook/developers-hub/use-cases/microtransactions-and-cashback-systems.md)** — スケーラブルなリワード・インセンティブシステム
- **[プライバシー保護アプリケーション](/gitbook/developers-hub/use-cases/privacy-preserving-applications.md)** — 匿名性の高いアプリケーションとサービス

### ビジネス機会

- **[Block Builder ビジネスガイド](/gitbook/developers-hub/intmax-block-builder/business-guide.md)** — インフラプロバイダー向けの運用インサイト
- **[ネットワーク参加](/gitbook/developers-hub/intmax-block-builder/block-builder-reference.md)** — ブロックバリデーションを通じたリワードの獲得

## セキュリティとパフォーマンス

### セキュリティ機能

- **[セキュリティとプライバシー](/gitbook/developers-hub/security-and-privacy/architecture-principles.md)** — Lean 定理証明器による形式検証
- **ゼロリーク** — トランザクションデータはオンチェーンに一切公開されない
- **耐障害設計** — リプレイ攻撃、遅延攻撃、検閲攻撃に対する保護

### パフォーマンス指標

- **[パフォーマンス概要](/gitbook/developers-hub/performance.md)** — 現在 約 7,500 TPS、EIP-4844 対応で 320,000+ TPS
- **効率性** — 送信者あたりわずか約 5 バイト（他のロールアップの 200〜300 バイトと比較）

## INTMAX の主な特徴

- **Privacy by Design** — トランザクションデータはオンチェーンに公開されず、最大限のプライバシーを実現
- **バッチ転送（Batch Transfer）** — 1 回のトランザクションで最大 63 人の受取人にトークンを送信
- **ステートレスアーキテクチャ（Stateless Architecture）** — 中央集権的な状態管理を排除したクライアント主体の設計
- **分散型運用** — 単一のシーケンサー（Sequencer）が不要。パーミッションレス（Permissionless）なブロック集約
- **EVM 互換性** — Transfer に特化しつつ、必要に応じて EVM 互換
- **低手数料** — オンチェーンデータ要件を最小化し、最大限のコスト効率を実現

## 統合ガイド

### dApp 開発者向け {#for-dapp-developers}

1. **[Client SDK](/gitbook/developers-hub/intmax-client-sdk/overview.md) から始める** — WebAssembly ベースの高パフォーマンス SDK
2. **[Use Cases](/gitbook/developers-hub/use-cases/payments.md) を探索** — 実用的な実装例
3. **トランザクションライフサイクルを理解** — 作成 → 署名 → ブロードキャストの流れ
4. **[VibeCoding ガイド](/gitbook/developers-hub/intmax-client-sdk/vibe-coding-for-general-prompt.md) を活用** — INTMAX + AI を使ったアイデア生成とプロトタイプ構築

### ウォレット開発者向け {#for-wallet-developers}

1. **[Wallet SDK](/gitbook/developers-hub/intmax-wallet-sdk/wallet-sdk-reference.md) を実装** — EIP-1193 スタイルの通信プロトコル

### インフラプロバイダー向け {#for-infra-providers}

1. **[Block Builder](/gitbook/developers-hub/intmax-block-builder/block-builder-reference.md) をデプロイ** — コアインフラコンポーネント
2. **[ビジネスガイド](/gitbook/developers-hub/intmax-block-builder/business-guide.md) を確認** — 運用と経済性に関するインサイト
