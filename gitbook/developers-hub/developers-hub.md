---
description: Developers Hub 概要と各セクションへのナビゲーション
icon: compass
---

# Developers Hub 概要

## INTMAX Network の開発者向け総合ガイド

— Welcome to the INTMAX Network Developers Hub —

{% hint style="info" %}
dApp開発、ウォレット構築、インフラのセットアップなど、INTMAX Network 上でアプリケーションを効率的に構築・統合・デプロイするための包括的なリソースとそれらの解説
{% endhint %}

## クイックスタート

### はじめに

* [**Core Concepts**](koakonseputo/rollup-architecture.md) — INTMAX のステートレス（Stateless）でプライバシーファーストな L2（Layer 2）アーキテクチャの理解
* [**Payment Lifecycle**](payment-lifecycle.md) — ZK proof を用いた Deposit・Transfer・Withdrawal の流れ
* [**INTMAX Nodes**](intmax-nodes/node-reference.md) — ネットワークを構成する各ノードタイプとその役割

### 開発ツール

* [**INTMAX Client SDK**](intmax-client-sdk/overview.md) — WebAssembly ベースの高パフォーマンス暗号処理 SDK
* [**INTMAX CLI**](intmax-cli.md) — CSV 対応で最大 63 件の Transfer をバッチ処理できるコマンドラインツール
* [**INTMAX Wallet SDK**](intmax-wallet-sdk/wallet-sdk-reference.md) — ウォレット ↔ dApp 統合のためのプロトコルと SDK
* [**コードリポジトリ**](intmax-wallet-sdk/code-repository.md) — INTMAX 各コンポーネントの公式 GitHub リポジトリ

### 統合パス

* [**dApp 開発者向け**](../#for-dapp-developers) — INTMAX 上でのアプリケーション構築を開始
* [**ウォレット開発者向け**](../#for-wallet-developers) — ウォレットへの INTMAX サポートの統合
* [**インフラプロバイダー向け**](../#for-infra-providers) — ネットワークノードのデプロイと運用

## ネットワークインフラ

### コアノード

* [**ノードリファレンス**](intmax-nodes/node-reference.md) — 全ノードタイプの概要とデプロイガイド
* [**Block Builder**](intmax-nodes/block-builder.md) — トランザクションをブロックに集約してリワードを獲得
* [**Validity Prover**](intmax-nodes/validity-prover.md) — Merkle Tree のホストと ZK proof の生成
* [**Withdrawal Server**](intmax-nodes/withdrawal-server.md) — Withdrawal リクエストの処理と L1 との同期
* [**Provers**](intmax-nodes/provers.md) — ネットワーク検証のための暗号プルーフ生成

### ブリッジインフラ

* [**Deposit Relayer**](intmax-nodes/deposit-relayer.md) — Ethereum から INTMAX への資産ブリッジ
* [**Withdrawal Relayer**](intmax-nodes/withdrawal-relayer.md) — Withdrawal のオンチェーン同期
* [**Withdrawal Aggregator**](intmax-nodes/withdrawal-aggregator.md) — L2 → L1 の Withdrawal Claim のバンドル
* [**Claim Aggregator**](intmax-nodes/claim-aggregator.md) — マイニングリワード Claim のバンドルと検証

### サポートサービス

* [**Store Vault Server**](intmax-nodes/store-vault-server.md) — プライベートデータのバックアップとリストア
* [**Indexer**](intmax-nodes/indexer.md) — ノードディスカバリーとデータフィードの提供
* [**スマートコントラクト**](intmax-nodes/smart-contracts.md) — INTMAX の L1 コントラクトスイート

## Use Cases とアプリケーション

### 実用アプリケーション

* [**決済**](use-cases/payments.md) — 高速かつ低コストなトークン Transfer
* [**マイクロトランザクションとキャッシュバック**](use-cases/microtransactions-and-cashback-systems.md) — スケーラブルなリワード・インセンティブシステム
* [**プライバシー保護アプリケーション**](use-cases/privacy-preserving-applications.md) — 匿名性の高いアプリケーションとサービス

### ビジネス機会

* [**Block Builder ビジネスガイド**](intmax-block-builder/business-guide.md) — インフラプロバイダー向けの運用インサイト
* [**ネットワーク参加**](intmax-block-builder/block-builder-reference.md) — ブロックバリデーションを通じたリワードの獲得

## セキュリティとパフォーマンス

### セキュリティ機能

* [**セキュリティとプライバシー**](sekyurititopuraibash/architecture-principles.md) — Lean 定理証明器による形式検証
* **ゼロリーク** — トランザクションデータはオンチェーンに一切公開されない
* **耐障害設計** — リプレイ攻撃、遅延攻撃、検閲攻撃に対する保護

### パフォーマンス指標

* [**パフォーマンス概要**](performance.md) — 現在 約 7,500 TPS、EIP-4844 対応で 320,000+ TPS
* **効率性** — 送信者あたりわずか約 5 バイト（他のロールアップの 200〜300 バイトと比較）

## INTMAX の主な特徴

* **Privacy by Design** — トランザクションデータはオンチェーンに公開されず、最大限のプライバシーを実現
* **バッチ転送（Batch Transfer）** — 1 回のトランザクションで最大 63 人の受取人にトークンを送信
* **ステートレスアーキテクチャ（Stateless Architecture）** — 中央集権的な状態管理を排除したクライアント主体の設計
* **分散型運用** — 単一のシーケンサー（Sequencer）が不要。パーミッションレス（Permissionless）なブロック集約
* **EVM 互換性** — Transfer に特化しつつ、必要に応じて EVM 互換
* **低手数料** — オンチェーンデータ要件を最小化し、最大限のコスト効率を実現

## 統合ガイド

### dApp 開発者向け <a href="#for-dapp-developers" id="for-dapp-developers"></a>

1. [**Client SDK**](intmax-client-sdk/overview.md) **から始める** — WebAssembly ベースの高パフォーマンス SDK
2. [**Use Cases**](use-cases/payments.md) **を探索** — 実用的な実装例
3. **トランザクションライフサイクルを理解** — 作成 → 署名 → ブロードキャストの流れ
4. [**VibeCoding ガイド**](intmax-client-sdk/vibe-coding-for-general-prompt.md) **を活用** — INTMAX + AI を使ったアイデア生成とプロトタイプ構築

### ウォレット開発者向け <a href="#for-wallet-developers" id="for-wallet-developers"></a>

1. [**Wallet SDK**](intmax-wallet-sdk/wallet-sdk-reference.md) **を実装** — EIP-1193 スタイルの通信プロトコル

### インフラプロバイダー向け <a href="#for-infra-providers" id="for-infra-providers"></a>

1. [**Block Builder**](intmax-block-builder/block-builder-reference.md) **をデプロイ** — コアインフラコンポーネント
2. [**ビジネスガイド**](intmax-block-builder/business-guide.md) **を確認** — 運用と経済性に関するインサイト
