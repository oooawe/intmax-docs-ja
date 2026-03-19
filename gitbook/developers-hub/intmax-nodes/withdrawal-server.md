---
icon: server
description: Withdrawal Server の役割と Withdrawal・Claim リクエストの処理の仕組み
---

# Withdrawal Server

## Withdrawal Server とは

Withdrawal Server は、INTMAX ネットワーク上でユーザーの Withdrawal リクエストと Claim リクエストを処理するノードです。リクエストの受付、プルーフの検証、送金の実行、およびユーザーへのトランザクション詳細の提供を管理します。

## 主な役割

- **Withdrawal リクエスト**
  - Withdrawal リクエストを受け付け、プルーフを検証し、リクエストされた金額を送金します。
- **Claim リクエスト**
  - Claim リクエストを処理し、プルーフを検証し、Claim された金額を送金します。
- **手数料の計算**
  - Withdrawal と Claim に関する手数料を計算し、ユーザーに情報を提供します。
- **情報の取得**
  - 認証済みユーザーに対して、Withdrawal と Claim の詳細情報を提供します。

## 動作の流れ

- ユーザーが Withdrawal Server に Withdrawal または Claim リクエストを送信します。
- Withdrawal Server がトランザクションのプルーフを検証し、有効であれば処理を実行します。
- 処理完了後、資金が受信者に送金されます。ユーザーはトランザクションの詳細と手数料情報を取得できます。

## 特徴

- **分散型**
  - 中央管理者なしに、ネットワーク全体で複数のノードが独立して動作します。
  - ユーザーは自身の Withdrawal Server ノードを運用し、プロトコルに基づくリワードを受け取れます。

## ユーザーができること

- 自身の Withdrawal Server ノードをデプロイし、ネットワーク運用に参加してプロトコルに基づくリワードを獲得できます。
- Withdrawal と Claim のリクエスト、トランザクション詳細の取得、現在の手数料体系の確認を API を通じて行えます。
