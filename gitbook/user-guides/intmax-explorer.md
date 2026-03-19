---
icon: magnifying-glass-chart
description: INTMAX Explorer の使い方とトランザクション検索
---

# INTMAX Explorer

## はじめに

**INTMAX Explorer** は、INTMAX Network 上で発生する Deposit、Withdrawal、ブロック送信などのアクティビティを可視化するツールです。主に以下の用途で使用します：

- INTMAX Network の現在の状態と過去のアクティビティの監視
- 自身の Deposit・Withdrawal 履歴の追跡
- ブロック送信のステータスと履歴の確認

### 主な Use Cases

**ユーザーの視点：**

- 自分の Deposit や Withdrawal が正常に処理されたかを確認

**Block Builder の視点：**

- 送信したブロックがネットワークに受け入れられたかを検証
- 投稿に成功したブロックと失敗したブロックの一覧を閲覧

### 対象読者

**INTMAX Network のユーザー：**

- Deposit、Withdrawal、Transfer を行ったことがあるユーザー
- 自分の操作がネットワーク上でどう処理されるかを理解したいユーザー

**Block Builder：**

- 生成・送信したブロックがネットワークに受け入れられたかを検証したい開発者やノードオペレーター

### 前提知識

このドキュメントは、以下の概念について基本的な理解があることを前提としています：

- トランザクション、ブロック、アドレスなどのブロックチェーンの基礎概念
- L1（Layer 1）と L2（Layer 2）の違い

### プライバシーに関する制限事項

プライバシーへの配慮から、Explorer では以下の情報は表示されません：

- **アドレスの残高：** あるアドレスが保有する ETH やトークンの残高は閲覧できません。
- **トランザクションの内容：** Transfer トランザクションの詳細は公開されません。
- **INTMAX オンチェーンアドレス：**
  - INTMAX への Deposit 先アドレス
  - INTMAX からの Withdrawal 元アドレス

これらの制限は、ユーザーのプライバシーを確保し、INTMAX のステートレス（Stateless）なゼロ知識アーキテクチャに沿ったものです。

## Home

INTMAX Explorer には以下からアクセスできます：

- **Mainnet:** https://explorer.intmax.io/
- **Testnet:** https://beta.testnet.explorer.intmax.io/

<figure><img src="assets/user-guides/intmax_explorer_10.webp" style={{ maxHeight: "450px" }} alt="INTMAX Explorer Home" /></figure>

## Blocks

### Validity

- **Valid：** ブロックの内容がプロトコルルールに従って検証され、正しいと判定されたことを示します。
- **Invalid：** ブロックの内容がプロトコルルールに違反しているため、拒否されたことを示します。このブロック内のすべてのトランザクションは無効となり、破棄されます。
- **Empty：** ブロックにトランザクションが含まれていないことを示します。

### Status

- **Proving：** ブロックは正常に送信され、現在その正当性を確認するための検証が行われています。
- **Completed：** ブロックの有効性が判定されました。有効であれば「Validity」フィールドに Valid と表示され、そうでなければ Invalid と表示されます。

### Block type

- **Type 0：** ユーザートランザクションを含まないブロック。通常、Deposit アクションを自動的に反映するために実行されます。
- **Type 1：** 送信者アドレスからの最初のトランザクションを含むブロック。送信者の初回オンチェーンアクティビティに使用される特殊なブロックタイプです。
- **Type 2：** 送信者アドレスからの 2 回目以降のトランザクションを含むブロック。Type 1 と比較して低い手数料で実行されるため、繰り返しのトランザクションがよりコスト効率的になります。

### ブロック一覧

以下の情報を含むブロックのリアルタイムページネーションリストが表示されます：

- Height（ブロック番号）
- Hash
- Timestamp
- トランザクション数
- Validity
- Status
- Block type

Status と Block type によるフィルタリングが可能です。

![INTMAX Explorer Home](assets/user-guides/intmax_explorer_20.webp)

### ブロック詳細

ブロックレベルの詳細情報を表示します：

- Status と Timestamp
- ブロック生成者のアドレス
- ブロックの有効性証明（ZKP）

ユーザーや Block Builder が特定のブロックの投稿成否を確認するのに役立ちます。

![INTMAX Explorer Block Details](assets/user-guides/intmax_explorer_30.webp)

## Deposits

### Status

- **Relayed：** リクエストはオンチェーンに送信されましたが、まだ処理中です。
- **Rejected：** Deposit が拒否されました。
- **Completed：** リクエストが正常に反映されました。Deposit された資金を INTMAX Network で使用できます。

### Deposit 一覧

すべての Deposit トランザクションが表示されます。カラムには以下が含まれます：

- Deposit ID
- Status
- Timestamp
- Token Type
- Amount
- TxHash

Status や日付でフィルタリングして、Deposit 履歴を効率的に監視できます。

![INTMAX Explorer Deposits List](assets/user-guides/intmax_explorer_40.webp)

### Deposit 詳細

特定の Deposit の詳細ビューで、以下が表示されます：

- Status と Timestamp
- 紐づく L1 トランザクションハッシュ

フィルタリングオプションにより、Deposit のフローを追跡できます。

![INTMAX Explorer Deposit Details](assets/user-guides/intmax_explorer_50.webp)

## Withdrawals

### Status

- **Relayed：** リクエストはオンチェーンに送信されましたが、まだ処理中です。
- **Rejected：** Withdrawal が拒否されました。
- **Completed：** リクエストが正常に反映されました。ETH および一部の ERC-20 トークンはユーザーが指定したアドレスに自動的に入金されるため、追加の操作は不要です。ただし、その他の ERC-20 トークンや NFT は、Web App のトランザクション画面から手動で Claim する必要があります。

### Withdrawal 一覧

すべての Withdrawal トランザクションが以下の詳細とともに表示されます：

- Withdrawal Hash
- Status
- Timestamp
- Token type
- Amount
- TxHash

フィルタリングオプションにより、Withdrawal のフローを追跡できます。

![INTMAX Explorer Withdrawals List](assets/user-guides/intmax_explorer_60.webp)

### Withdrawal 詳細

Withdrawal トランザクションの詳細情報を表示します：

- Status と Timestamp
- 紐づく L1 トランザクションハッシュ

<figure>
  <img src="assets/user-guides/intmax_explorer_70.webp" alt="INTMAX Explorer Withdrawal Details Relayed" />
  <img src="assets/user-guides/intmax_explorer_80.webp" alt="INTMAX Explorer Withdrawal Details Completed" />
</figure>

## 検索機能

検索機能では、以下を入力してナビゲーションできます：

- Block height
- Block hash
- Deposit Hash
- Withdrawal Hash

この機能により、ブロック詳細、Deposit 詳細、Withdrawal 詳細に直接アクセスできます。

![INTMAX Explorer Search](assets/user-guides/intmax_explorer_90.webp)
