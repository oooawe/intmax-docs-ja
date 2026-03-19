---
icon: rectangle-terminal
description: INTMAX CLI の概要と主要機能
---

# INTMAX CLI

## 概要

INTMAX CLI は、zk-Rollup ベースの L2（Layer 2）ソリューションである INTMAX Network とシームレスに連携するために設計された公式コマンドラインインターフェースです。包括的な管理機能を備え、デジタル資産を正確かつ簡単に取り扱えます。

主な機能として、CSV ファイルからのバッチ転送（Batch Transfer）によるアセット管理があり、一度の操作で最大 63 件のトランザクションを効率的に処理して手数料を大幅に削減できます。また、トランザクションステータスの監視も容易に行え、資産の透明性と管理性を高めます。

さらに、INTMAX CLI は Privacy Mining への参加を効率化し、マイニングステータスの確認、ITX トークンでのリワードの Claim、マイニング固有の Deposit 管理を直感的なコマンドで操作できます。

新しいロールアップ鍵の生成から、既存の Ethereum 秘密鍵（Private Key）からの安全な鍵導出まで、INTMAX CLI は利便性とセキュリティの両立を重視しています。ユーザー中心の設計により、INTMAX エコシステムにおける信頼性の高い効率的なインタラクションに不可欠なツールです。

[GitHub で見る →](https://github.com/InternetMaximalism/intmax2/tree/main/cli)

## 主な特徴

- **鍵生成** — 新しいロールアップ鍵の生成、または既存の Ethereum 秘密鍵からの導出
- **Deposit** — ETH や任意の ERC-20/721/1155 トークンをロールアップに Deposit
- **Withdrawal** — INTMAX Network から Ethereum へのコマンドラインでの Withdrawal 実行（進捗追跡機能付き）
- **Transfer** — CSV ファイルから一度の操作で最大 63 件のトランザクションを処理するバッチ転送機能により、手数料を削減
- **マイニングとリワードの Claim** — INTMAX Network は Privacy Mining 参加者に ITX トークンのリワードを付与。CLI から直接ステータス確認とリワードの Claim が可能
- **マイニング専用 Deposit タイプ** — マイニング用に許容される Deposit 額面（0.1 / 1 / 10 / 100 ETH）を確認・検証
