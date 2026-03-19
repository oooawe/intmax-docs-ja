---
icon: book-open
description: INTMAX WalletSDK の JSON-RPC API メソッド一覧と分類
---

# API メソッド

このセクションでは、INTMAX WalletSDK の JSON-RPC API のインタラクティブなリファレンスを提供します。この API は、標準的な Ethereum メソッドをベースに INTMAX WalletSDK 固有の拡張を加えたもので、dApp へのシームレスな統合を目的として設計されています。

`INTMAX WalletSDK` プロトコルでは、3 種類の JSON-RPC メソッドを定義しています。

- **[notice](#notice-メソッド)**：ウォレットからの通知メッセージ
- **[approval](#approval-メソッド)**：ウォレットに承認を要求するメソッド
- **[readonly](#readonly-メソッド)**：dApp 側で解決される読み取り専用メソッド

## Notice メソッド

ウォレットからの通知を表す、やや特殊なメソッドタイプです。ウォレットが暗黙的に発火し、dApp 側に通知されます。INTMAX WalletSDK プロトコルで定義されたメソッド以外では使用されません。

対象メソッド：

- [INTMAX Ready](intmax-ready)
- [INTMAX Connect](intmax-connect)

## Approval メソッド

署名（Signature）などユーザーの承認を必要とするメソッドです。

以下のメソッドは、dApp からウォレットに対してユーザーの承認を要求します。各 EIP のスキーマを継承しています。

- [Request Wallet Account](request-wallet-account)
- [Sign Message](sign-message)
- [Sign Typed Data](sign-typed-data)
- [Sign Transaction](sign-transaction)
- [Send Transaction](send-transaction)
- [Add Chain to Wallet](add-chain-to-wallet)
- [Watch Wallet Asset](watch-wallet-asset)

## Readonly メソッド

`eip155/eth_accounts` のような読み取り専用メソッドです。通常は SDK が dApp 側でキャッシュするため、ウォレットへのリクエストは発生しません。ただし、これらは単なるメソッドであるため、ウォレット側で処理することも可能です。

以下のメソッドは、SDK が dApp 側で解決することを想定しています。ウォレットへのリクエストは行われません。なお、「readonly」という命名は将来変更される可能性があります。

- [Get Accounts](get-accounts)
- [Get Chain ID](get-chain-id)
- [Switch Chain](switch-chain)
