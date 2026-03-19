---
icon: server
description: Deposit Relayer の役割と Ethereum から INTMAX への Deposit 中継の仕組み
---

# Deposit Relayer

## Deposit Relayer とは

Deposit Relayer は、ユーザーが Ethereum ネットワーク上で行った Deposit（ETH、ERC-20、ERC-721、ERC-1155 トークン）を INTMAX ネットワークに反映するノードです。

## 主な役割

- **Deposit 情報の中継**
  - Ethereum ネットワークからの Deposit データを定期的に INTMAX ネットワークに中継します。

## 動作の仕組み

- Deposit Relayer は、ユーザーの Deposit トランザクションを定期的に Scroll ネットワーク上の Rollup コントラクトに中継します。
  - データは [Scroll Messenger](https://docs.scroll.io/en/developers/l1-and-l2-bridging/the-scroll-messenger/) を通じて Rollup コントラクトに配信されます。
  - Rollup コントラクトに配信されると、INTMAX ブロックの生成後にユーザーの残高に反映されます。

## 特徴

このノードは中央管理者に依存せず、複数のノードで独立して動作します。ユーザーも自身の Deposit Relayer ノードを運用できますが、リワードは提供されません。

## ユーザーができること

ユーザーは自身の Deposit Relayer ノードを構築・運用してプロトコルに参加できます。ただし、参加に対するリワードは提供されません。
