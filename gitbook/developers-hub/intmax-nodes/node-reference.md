---
icon: users-gear
description: INTMAX ネットワークを構成するノード・コントラクト・UI の全体像
---

# ノードリファレンス

本ドキュメントでは、INTMAX システム全体のコンポーネントとその役割を説明します。コンポーネントは、ユーザーインターフェース、コントラクト、ノードの 3 つのカテゴリに分類されます。

- **ユーザーインターフェースの説明**
  - ユーザーがネットワークと対話するためのインターフェースを説明します。CLI（Command Line Interface）と Intmax Web App について、ネットワークの操作方法を解説します。
- **スマートコントラクトの説明**
  - ネットワーク内の各コントラクトの役割を説明します。
  - ネットワークがオンチェーンデータをどのように活用しているかを理解するのに役立ちます。
- **ノードの説明**
  - ネットワーク内の各ノードの役割を説明します。
  - ネットワークの構造を理解するのに役立ちます。

## コンポーネントの依存関係

### 概念図

![概念図](assets/developers-hub/node.webp)

## 主な機能

### 複数トークンのバッチ転送

INTMAX は複数トークンのバッチ転送（Batch Transfer）をネイティブにサポートしています。ユーザーは追加コストなしで、最大 63 人の異なる受信者に同時にトークンを送れます。1 回のバッチ転送で、受信者ごとに異なる種類のトークンを送ることも可能です。

INTMAX ネットワーク上のトランザクションでは、受信者の情報が外部の観察者に公開されることはありません。

### Ethereum ネットワークとの Deposit・Withdrawal

INTMAX ネットワークは Ethereum ネットワークからの Deposit と Withdrawal をサポートしています。Deposit 時には、どの INTMAX アドレスが資金を受け取ったかを知ることができるのは受信者本人だけです。同様に、Withdrawal 時にも、資金の送信元となる INTMAX アドレスは開示されません。

Deposit 時には AML（Anti-Money Laundering）チェックが行われ、リスクのある資産が INTMAX ネットワーク内で流通するのを防止します。

### Privacy Mining

Privacy Mining の目的は、プライバシープロトコルの匿名セットを拡大・維持することです。ユーザーは指定された量の ETH を一定期間 INTMAX ネットワーク内に Deposit することで、プライバシーに貢献し、リワードを獲得できます。

## ユーザーインターフェース

### Intmax Web App（推奨）

Intmax Web App は、ユーザーが INTMAX ネットワーク上でアカウントを作成し、トークンの Deposit・Transfer・Withdrawal などの操作を行うための UI を提供します。

Privacy Mining 機能にも対応しています。Intmax Web App はウェブサイトおよびモバイルアプリケーションとして利用できます。

- [メインネットアプリを開く](https://app.intmax.io/)

- [テストネットアプリを開く](https://beta.testnet.app.intmax.io/)

### CLI

ユーザーがターミナルから INTMAX ネットワークを操作するための CLI プログラムです。アカウントの作成、トークンの Deposit・Transfer・Withdrawal などの操作コマンドを提供します。

以下のソースコードからアクセスできます。

[INTMAX CLI を見る →](../intmax-cli.md)

### コントラクト

INTMAX ネットワークは Ethereum ネットワーク上に構築された L2（Layer 2）ソリューションです。流動性は Ethereum 上に残り、ブロックデータの保存には zk-Rollup の一種である Scroll ネットワークを利用することで、低コストと高いセキュリティを実現しています。これら 2 つのネットワーク上にデプロイされたスマートコントラクトが、INTMAX ネットワークの基盤を形成しています。

[スマートコントラクトを見る →](./smart-contracts)

## ノード

### Block Builder

Block Builder ノードは、ユーザーからの Transfer リクエストを集約してブロックを作成し、ネットワークに反映します。

このノードは**分散型**であり、ネットワーク全体にデプロイされています。各ノードは独立して動作し、他の Block Builder ノードと同期する必要はありません。

[Block Builder を見る →](./block-builder)

### Store Vault Server

ユーザーが個別に保存すべきデータをバックアップし、複数のデバイスから INTMAX Wallet や CLI を利用する際にアクセスできるようにします。ユーザーデータを安全かつ効率的にバックアップし、必要時の復元プロセスを管理するノードです。

[Store Vault Server を見る →](./store-vault-server)

### Deposit Relayer

Liquidity コントラクトからの Deposit リクエストを Rollup コントラクトに中継します。

[Deposit Relayer を見る →](./deposit-relayer)

### Withdrawal Server

Withdrawal・Claim リクエストの受付状況を管理し、認証済みユーザーに Withdrawal と Claim に関する情報を提供するノードです。

[Withdrawal Server を見る →](./withdrawal-server)

### Withdrawal Aggregator

INTMAX ネットワークから Ethereum への Withdrawal リクエストを処理します。ユーザーの Withdrawal リクエストを受け付け、必要な検証と処理を行い、Liquidity コントラクトへの反映プロセスを管理するノードです。

[Withdrawal Aggregator を見る →](./withdrawal-aggregator)

### Claim Aggregator

INTMAX ネットワーク上で発生したマイニングリワードのリクエストを処理するノードです。ユーザーから受け取ったマイニングリワードリクエストに対して必要な検証と処理を行い、オンチェーンへの反映プロセスを管理します。

[Claim Aggregator を見る →](./claim-aggregator)

### Withdrawal Relayer

Rollup コントラクトに送信された Withdrawal 情報を Liquidity コントラクトに反映します。

[Withdrawal Relayer を見る →](./withdrawal-relayer)

### Indexer

ユーザーが Block Builder や Validity Prover を見つけるための情報を提供します。現在アクティブなノードの一覧を管理し、接続先のノードを推奨します。

[Indexer を見る →](./indexer)

### Validity Prover

Validity Prover は、INTMAX ネットワーク上の分散型ノードであり、INTMAX ブロックに関連する Merkle Tree と ZKP を安全に保存・提供する役割を担います。これらのプルーフによってトランザクションとブロックの有効性が検証され、ネットワーク運用の整合性が保証されます。

[Validity Prover を見る →](./validity-prover)

### Provers

INTMAX ネットワークでは、ZKP（Zero-Knowledge Proof）を使ってユーザーの残高を検証し、十分な資金があることと Withdrawal 条件を満たしていることを確認しています。

[Provers を見る →](./provers)
