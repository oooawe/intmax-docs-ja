---
icon: server
description: Withdrawal Relayer の役割と INTMAX から Ethereum への Withdrawal 実行の仕組み
---

# Withdrawal Relayer

## Withdrawal Relayer とは

Withdrawal Relayer は、INTMAX ネットワークから Ethereum ネットワークへの Withdrawal を安全に処理する分散型ノードです。Rollup コントラクトを通じて送信されたユーザーリクエストに基づき、ETH、ERC-20、ERC-721、ERC-1155 などのトークンの Withdrawal を実行します。

## 主な役割

- **Withdrawal の実行**
  - Scroll Messenger を介して Rollup コントラクトから受け取った Withdrawal リクエストを処理します。
  - Ethereum 上の Liquidity コントラクトの `processWithdrawals` 関数を呼び出してトークンを送金します。

## 動作の流れ

- Withdrawal Relayer は、Scroll Messenger API を継続的に監視し、Withdrawal リクエストと Claim リクエストを検知します。
- Withdrawal の指示を受け取ると、Ethereum 上でトランザクションを安全に実行し、リクエストされたトークンをユーザーのアドレスに送金します。
- ETH または特定の ERC-20 トークンの直接送金が失敗した場合、ユーザーは Liquidity コントラクト上で Claim トランザクションを実行してトークンを受け取る必要があります。
- その他の ERC-20、ERC-721、ERC-1155 トークンについては、ユーザーが Liquidity コントラクト上で Claim トランザクションを手動で実行する必要があります。

## 特徴

- **分散型**
  - Withdrawal Relayer ノードはネットワーク全体に分散して独立に動作します。
  - ユーザーは自身の Withdrawal Relayer ノードをデプロイし、プロトコルに基づくリワードを獲得できます。
- **高可用性**
  - ネットワーク内で少なくとも 1 つのノードが常に稼働し、継続的な可用性を確保します。
- **競合管理**
  - 複数のノードが同時にアクションを試みる場合、Ethereum ノードアドレスによる処理順序を用いて競合を効果的に処理します。
- **障害復旧**
  - ダウンタイムや障害が発生した場合、自動的に運用を再開します。
- **安全なアップデート**
  - ソフトウェアパッチやアップデートを安全かつ効率的に適用します。

## セキュリティ対策

- **リアルタイムログ監視**
  - ログを継続的に監視し、不正アクセスや異常な活動を迅速に検知・対応します。
- **秘密鍵（Private Key）のセキュリティ**
  - 秘密鍵へのアクセスは厳格に管理され、必要なユーザーやプロセスに限定されます。
  - 秘密鍵は暗号化されたデータベースまたは物理的に保護された場所に安全に保管されます。

## ユーザーができること

- 自身の Withdrawal Relayer ノードをセットアップ・運用してネットワーク運用に貢献し、プロトコルに基づくリワードを受け取れます。
- 直接送金で問題が発生した場合、Liquidity コントラクトを通じてトークンを Claim することで間接的にやり取りします。
