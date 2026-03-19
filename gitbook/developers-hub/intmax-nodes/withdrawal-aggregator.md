---
icon: server
description: Withdrawal Aggregator の役割と Withdrawal リクエストの集約・検証の仕組み
---

# Withdrawal Aggregator

## Withdrawal Aggregator とは

Withdrawal Aggregator は、INTMAX ネットワークから Ethereum ネットワークへの Withdrawal リクエストを処理する分散型ノードです。ユーザーの Withdrawal リクエストの検証、準備、および Ethereum ブロックチェーンへの安全な反映を管理します。

## 主な役割

- **Withdrawal リクエストの処理**
  - ユーザーからの Withdrawal リクエストを受け付け、検証し、情報を安全に保存します。
- **Withdrawal の集約と送信**
  - ユーザーリクエストに基づいて ZKP を生成し、集約した Withdrawal データを Ethereum ネットワークに送信します。
- **Withdrawal プルーフの提供**
  - ユーザーの Withdrawal リクエストに対して Merkle Proof を提供し、安全で検証可能な Withdrawal を実現します。

## 動作の流れ

- ユーザーが Transfer の詳細と必要なプルーフを含む Withdrawal リクエストを送信します。
- Withdrawal Aggregator が各リクエストのプルーフと残高を検証し、データを安全に保存します。
- 十分なリクエストが蓄積されるか、指定された時間間隔が経過すると、ZKP を作成し、集約された Withdrawal 情報を Scroll ネットワーク経由で Ethereum に送信します。
- 送信完了後、ユーザーは Withdrawal を確定するための Merkle Proof を受け取ります。

## 特徴

- **分散型**
  - ノードは中央管理なしにネットワーク全体で独立して動作します。ユーザーは自身のノードを運用できますが、プロトコルによるリワードは提供されません。
- **高可用性**
  - 最小限のダウンタイムで継続的なサービスを確保します。
- **スケーラブル**
  - 増加するリクエストを効率的に処理できます。
- **耐障害性**
  - 障害や停止が発生しても、シームレスに運用を再開できます。
- **高速レスポンス**
  - ユーザーリクエストに迅速に応答します。

## ユーザーができること

- 自身の Withdrawal Aggregator ノードをセットアップ・運用できます。
- Withdrawal リクエストを送信し、Ethereum 上で資金を受け取るために必要なプルーフを取得できます。
