---
description: INTMAX WalletSDK の JSON-RPC API メソッド一覧と分類
icon: book-open
---

# API メソッド

このセクションでは、INTMAX WalletSDK の JSON-RPC API のインタラクティブなリファレンスを提供します。この API は、標準的な Ethereum メソッドをベースに INTMAX WalletSDK 固有の拡張を加えたもので、dApp へのシームレスな統合を目的として設計されています。

`INTMAX WalletSDK` プロトコルでは、3 種類の JSON-RPC メソッドを定義しています。

* [**notice**](../../../../#notice-methods)：ウォレットからの通知メッセージ
* [**approval**](../../../../#approval-methods)：ウォレットに承認を要求するメソッド
* [**readonly**](../../../../#readonly-methods)：dApp 側で解決される読み取り専用メソッド

## Notice メソッド <a href="#notice-methods" id="notice-methods"></a>

ウォレットからの通知を表す、やや特殊なメソッドタイプです。ウォレットが暗黙的に発火し、dApp 側に通知されます。INTMAX WalletSDK プロトコルで定義されたメソッド以外では使用されません。

対象メソッド：

* [INTMAX Ready](intmax-ready.md)
* [INTMAX Connect](intmax-connect.md)

## Approval メソッド <a href="#approval-methods" id="approval-methods"></a>

署名（Signature）などユーザーの承認を必要とするメソッドです。

以下のメソッドは、dApp からウォレットに対してユーザーの承認を要求します。各 EIP のスキーマを継承しています。

* [Request Wallet Account](request-wallet-account.md)
* [Sign Message](sign-message.md)
* [Sign Typed Data](sign-typed-data.md)
* [Sign Transaction](sign-transaction.md)
* [Send Transaction](send-transaction.md)
* [Add Chain to Wallet](add-chain-to-wallet.md)
* [Watch Wallet Asset](watch-wallet-asset.md)

## Readonly メソッド <a href="#readonly-methods" id="readonly-methods"></a>

`eip155/eth_accounts` のような読み取り専用メソッドです。通常は SDK が dApp 側でキャッシュするため、ウォレットへのリクエストは発生しません。ただし、これらは単なるメソッドであるため、ウォレット側で処理することも可能です。

以下のメソッドは、SDK が dApp 側で解決することを想定しています。ウォレットへのリクエストは行われません。なお、「readonly」という命名は将来変更される可能性があります。

* [Get Accounts](get-accounts.md)
* [Get Chain ID](get-chain-id.md)
* [Switch Chain](switch-chain.md)
