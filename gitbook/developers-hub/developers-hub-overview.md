---
icon: bookmark
description: Developers Hub の概要と各セクションへのナビゲーション
---

# Developers Hub 概要

INTMAX Network の開発者向け総合ガイド
— Welcome to the INTMAX Network Developers Hub —

dApp 開発、ウォレット構築、インフラのセットアップなど、INTMAX Network 上でアプリケーションを効率的に構築・統合・デプロイするための包括的なリソースとステップバイステップの手順を提供します。

## クイックスタート

### はじめに

- **[Core Concepts](/developers-hub/core-concepts/)** — INTMAX のステートレス（Stateless）でプライバシーファーストな L2（Layer 2）アーキテクチャの理解
- **[Payment Lifecycle](./payment-lifecycle.md)** — ZK proof を用いた Deposit・Transfer・Withdrawal の流れ
- **[INTMAX Nodes](/developers-hub/intmax-nodes)** — ネットワークを構成する各ノードタイプとその役割

### 開発ツール

- **[INTMAX Client SDK](/developers-hub/intmax-client-sdk)** — WebAssembly ベースの高パフォーマンス暗号処理 SDK
- **[INTMAX CLI](./intmax-cli.md)** — CSV 対応で最大 63 件の Transfer をバッチ処理できるコマンドラインツール
- **[INTMAX Wallet SDK](/developers-hub/intmax-wallet-sdk)** — ウォレット ↔ dApp 統合のためのプロトコルと SDK
- **[コードリポジトリ](./code-repository.md)** — INTMAX 各コンポーネントの公式 GitHub リポジトリ

### 統合パス

- **[dApp 開発者向け](#dapp-開発者向け)** — INTMAX 上でのアプリケーション構築を開始
- **[ウォレット開発者向け](#ウォレット開発者向け)** — ウォレットへの INTMAX サポートの統合
- **[インフラプロバイダー向け](#インフラプロバイダー向け)** — ネットワークノードのデプロイと運用

## ネットワークインフラ

### コアノード

- **[ノードリファレンス](./intmax-nodes/node-reference.md)** — 全ノードタイプの概要とデプロイガイド
- **[Block Builder](./intmax-nodes/block-builder.md)** — トランザクションをブロックに集約してリワードを獲得
- **[Validity Prover](./intmax-nodes/validity-prover.md)** — Merkle Tree のホストと ZK proof の生成
- **[Withdrawal Server](./intmax-nodes/withdrawal-server.md)** — Withdrawal リクエストの処理と L1 との同期
- **[Provers](./intmax-nodes/provers.md)** — ネットワーク検証のための暗号プルーフ生成

### ブリッジインフラ

- **[Deposit Relayer](./intmax-nodes/deposit-relayer.md)** — Ethereum から INTMAX への資産ブリッジ
- **[Withdrawal Relayer](./intmax-nodes/withdrawal-relayer.md)** — Withdrawal のオンチェーン同期
- **[Withdrawal Aggregator](./intmax-nodes/withdrawal-aggregator.md)** — L2 → L1 の Withdrawal Claim のバンドル
- **[Claim Aggregator](./intmax-nodes/claim-aggregator.md)** — マイニングリワード Claim のバンドルと検証

### サポートサービス

- **[Store Vault Server](./intmax-nodes/store-vault-server.md)** — プライベートデータのバックアップとリストア
- **[Indexer](./intmax-nodes/indexer.md)** — ノードディスカバリーとデータフィードの提供
- **[スマートコントラクト](./intmax-nodes/smart-contracts.md)** — INTMAX の L1 コントラクトスイート

## Use Cases とアプリケーション

### 実用アプリケーション

- **[決済](./use-cases/payments.md)** — 高速かつ低コストなトークン Transfer
- **[マイクロトランザクションとキャッシュバック](./use-cases/microtransactions-and-cashback-systems.md)** — スケーラブルなリワード・インセンティブシステム
- **[プライバシー保護アプリケーション](./use-cases/privacy-preserving-applications.md)** — 匿名性の高いアプリケーションとサービス

### ビジネス機会

- **[Block Builder ビジネスガイド](./intmax-block-builder/business-guide.md)** — インフラプロバイダー向けの運用インサイト
- **[ネットワーク参加](/developers-hub/intmax-block-builder)** — ブロックバリデーションを通じたリワードの獲得

## セキュリティとパフォーマンス

### セキュリティ機能

- **[セキュリティとプライバシー](/developers-hub/security-and-privacy)** — Lean 定理証明器による形式検証
- **ゼロリーク** — トランザクションデータはオンチェーンに一切公開されない
- **耐障害設計** — リプレイ攻撃、遅延攻撃、検閲攻撃に対する保護

### パフォーマンス指標

- **[パフォーマンス概要](./performance.md)** — 現在 約 7,500 TPS、EIP-4844 対応で 320,000+ TPS
- **効率性** — 送信者あたりわずか約 5 バイト（他のロールアップの 200〜300 バイトと比較）

## INTMAX の主な特徴

- **Privacy by Design** — トランザクションデータはオンチェーンに公開されず、最大限のプライバシーを実現
- **バッチ転送（Batch Transfer）** — 1 回のトランザクションで最大 63 人の受取人にトークンを送信
- **ステートレスアーキテクチャ（Stateless Architecture）** — 中央集権的な状態管理を排除したクライアント主体の設計
- **分散型運用** — 単一のシーケンサー（Sequencer）が不要。パーミッションレス（Permissionless）なブロック集約
- **EVM 互換性** — Transfer に特化しつつ、必要に応じて EVM 互換
- **低手数料** — オンチェーンデータ要件を最小化し、最大限のコスト効率を実現

## 統合ガイド

### dApp 開発者向け

1. **[Client SDK](/developers-hub/intmax-client-sdk) から始める** — WebAssembly ベースの高パフォーマンス SDK
2. **[Use Cases](/developers-hub/use-cases) を探索** — 実用的な実装例
3. **トランザクションライフサイクルを理解** — 作成 → 署名 → ブロードキャストの流れ
4. **[VibeCoding ガイド](/developers-hub/intmax-client-sdk/vibe-coding-for-general-prompt.md) を活用** — INTMAX + AI を使ったアイデア生成とプロトタイプ構築

### ウォレット開発者向け

1. **[Wallet SDK](/developers-hub/intmax-wallet-sdk) を実装** — EIP-1193 スタイルの通信プロトコル

### インフラプロバイダー向け

1. **[Block Builder](/developers-hub/intmax-block-builder) をデプロイ** — コアインフラコンポーネント
2. **[ビジネスガイド](./intmax-block-builder/business-guide.md) を確認** — 運用と経済性に関するインサイト
