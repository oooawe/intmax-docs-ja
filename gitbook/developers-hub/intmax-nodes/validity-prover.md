---
icon: brain-circuit
description: Validity Prover の役割と Merkle Tree・ZKP の保存・提供の仕組み
---

# Validity Prover

## Validity Prover とは

Validity Prover は、INTMAX ネットワーク上の分散型ノードであり、INTMAX ブロックに関連する Merkle Tree と ZKP を安全に保存・提供する役割を担います。これらのプルーフによってトランザクションとブロックの有効性が検証され、ネットワーク運用の整合性が保証されます。

## 主な役割

- **Merkle Tree と ZKP の保存**
  - 新しい INTMAX ブロックが作成されるたびに、Validity Prover が Merkle Tree のプルーフと ZKP を生成します。
  - これらのプルーフを安全に保存し、必要に応じて更新します。
- **ユーザーへのプルーフ提供**
  - 保存している Merkle Tree と ZKP を、ユーザーのリクエストに応じて提供します。

## 動作の仕組み

- ブロックが作成され Rollup コントラクトに投稿されると、Validity Prover が Merkle Proof と ZKP を生成・検証し、安全に保存します。
- ユーザーは、トランザクションの検証やアカウント詳細の確認に必要なプルーフをリクエストできます。

## 特徴

各 Validity Prover ノードは独立して動作します。ユーザーは自身のノードをデプロイできますが、プロトコルによるリワードは提供されません。

## ユーザーができること

ユーザーは Validity Prover を通じて INTMAX ネットワークの現在の状態を確認できます。主に、REST API を使って残高証明（Balance Proof）の生成に必要なデータを取得できます。
