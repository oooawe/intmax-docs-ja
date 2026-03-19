---
icon: brain-circuit
description: INTMAX で使用される各種 ZKP 生成ノードの概要と役割
---

# Provers

## Provers とは

INTMAX ネットワークでは、ZKP（Zero-Knowledge Proof）を使ってユーザーの残高を検証し、十分な資金があることと Withdrawal 条件を満たしていることを確認しています。

本ドキュメントでは、INTMAX で使用される各種 ZKP を生成するノードについて説明します。

## Balance Prover

ユーザーがアカウント残高の暗号学的な証明を作成するためのサーバーです。サーバーへのリクエストは暗号化され、データは安全な機密ストレージ内でのみ復号・処理されます。そのため、サーバー管理者であってもユーザーの残高やトランザクション履歴を閲覧することはできません。

## Validity Prover

INTMAX ブロックチェーンの状態の正しさを確認するプルーフを生成するサーバーです。

## Aggregator Prover

ユーザーからの Withdrawal（Claim）リクエストを 1 つの Withdrawal（Claim）プルーフに集約するサーバーです。異なるユーザーからの複数のリクエストを 1 つの統合プルーフにまとめることができます。

## Gnark Prover

集約された Withdrawal（Claim）プルーフを Solidity で検証可能なプルーフに変換するサーバーです。Withdrawal Aggregator が集約プルーフを Gnark Prover に送信し、Solidity で検証可能なプルーフを受け取った後、そのプルーフを直接スマートコントラクトに送信します。
