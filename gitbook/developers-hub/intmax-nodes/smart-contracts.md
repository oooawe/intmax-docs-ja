---
icon: file-contract
description: INTMAX を構成するスマートコントラクトの役割と機能の一覧
---

# スマートコントラクト

## 概要

INTMAX ネットワークは Ethereum ネットワーク上に構築された L2（Layer 2）ソリューションです。流動性は Ethereum 上に残り、ブロックデータの保存には zk-Rollup の一種である Scroll ネットワークを利用することで、低コストと高いセキュリティを実現しています。これら 2 つのネットワーク上にデプロイされたスマートコントラクトが、INTMAX ネットワークの基盤を形成しています。

本ドキュメントでは、INTMAX に関連するスマートコントラクトの役割を説明します。

## Rollup

INTMAX ブロックに関連する操作を管理するスマートコントラクトです。Scroll 上にデプロイされています。

- **ブロックの投稿** — ブロックの投稿を可能にします。
- **Deposit の処理** — Liquidity コントラクトからの Deposit を Rollup の状態に統合します。
- **ペナルティ手数料の管理** — レート制限に関するペナルティ手数料の Withdrawal と調整を可能にします。

## Liquidity

Ethereum ネットワークと INTMAX ネットワーク間の資産移動を管理するコントラクトです。ユーザーの Deposit・Withdrawal 操作を処理し、資産の状態を管理します。Ethereum 上にデプロイされています。

- **資産の Deposit** — ユーザーは Ethereum から INTMAX にトークン（ネイティブトークン、ERC-20、ERC-721、ERC-1155）を Deposit できます。
- **AML チェック** — Deposit 時に AML チェックを実施し、コンプライアンス基準を満たさないアドレスからの Deposit を拒否します。
- **資産の Withdrawal** — INTMAX から Withdrawal されたトークンを Ethereum 上のユーザーに送金します。
- **Deposit の管理** — Deposit されたトークンはキューに保存されます。Deposit Relayer がこの情報をまとめて INTMAX ネットワークに中継します。
- **Contribution の記録** — Withdrawal などネットワーク維持に必要なトランザクションを実行したアドレスを記録します。

## Withdrawal

INTMAX ネットワークから Ethereum への Withdrawal プロセスを管理し、Withdrawal プルーフの検証と直接 Withdrawal 対象のトークンインデックスの管理を行うコントラクトです。Scroll 上にデプロイされています。

- **Withdrawal リクエストの送信** — INTMAX ネットワークからの Withdrawal プルーフの送信と検証を可能にします。
- **直接 Withdrawal 対象トークンの管理** — 直接 Withdrawal 処理の対象となるトークンインデックスを管理します。

## Claim

Privacy Mining のリワードを Claim するためのコントラクトです。Withdrawal コントラクトと同じ手順に従い、Ethereum 上で ITX トークンを配布します。Scroll 上にデプロイされています。

- **Claim プルーフの送信** — INTMAX ネットワークからの Claim プルーフを送信します。
- **直接 Withdrawal 対象トークンの管理** — 直接 Withdrawal 対象のトークンを管理します。

## Block Builder Registry

ハートビート信号と URL を発信することでアクティブな Block Builder のレジストリを管理するコントラクトです。Block Builder が自身の稼働状態を公開するために使用されます。Scroll 上にデプロイされています。

- **ハートビートの発信** — Block Builder が定期的にアクティブであることを通知できるようにします。

## L1/L2 Contribution

ユーザーの Contribution を管理し、それぞれの割り当てとウェイトを追跡するコントラクトです。ユーザーの Contribution に基づいてリワードを計算・配布するために使用されます。Ethereum と Scroll の両方にデプロイされています。

- **Contribution の記録** — 特定の期間におけるユーザーの Contribution を関連タグ付きで記録します。
- **期間管理** — Contribution 記録のための定期的な増分とリセットを管理します。
- **ウェイト管理** — 各期間のさまざまなタグに対してウェイトを割り当て・管理します。

## Permitter Contract

Predicate と呼ばれるサービスを利用して AML チェックの検証と、トランザクションが正当なマイニング活動に基づくものかどうかの判定を行うコントラクトです。ユーザーが資金を Deposit する際に Liquidity コントラクトから呼び出されます。事前に定められたポリシーに準拠しないアドレスからの Deposit は拒否されます。Ethereum 上にデプロイされています。

- **ポリシー検証** — 事前に定められたポリシーに準拠しないアドレスからの Deposit は許可されません。これには AML 検証と正当なマイニング活動の検証という 2 種類のチェックが含まれます。
