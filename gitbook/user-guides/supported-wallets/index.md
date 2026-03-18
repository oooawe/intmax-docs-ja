---
icon: wallet
description: INTMAX が対応しているウォレットとブラウザの一覧
---

# 対応ウォレット

本サービスでは、ユーザーが安全かつ簡単にブロックチェーンネットワークへ接続できるよう、以下のブラウザとウォレットに対応しています。お使いのウォレットが以下の一覧に含まれていれば、トランザクションの署名やアセット管理をスムーズに行えます。

一覧に記載されていないウォレットの使用は推奨しません。本サービスで正常に動作しない可能性があり、アセットの喪失につながるおそれがあります。特に、INTMAX Wallet も現時点では対応していません。

対応ウォレットの一覧は定期的に更新される場合があります。最新情報はこのページをご確認ください。

## 対応ブラウザ

推奨ブラウザは以下のとおりです：

- **Mac：** Safari、Google Chrome
- **Windows：** Google Chrome
- **Android：** Google Chrome
- **iOS：** Safari

## 対応ウォレット

| アプリ名 | 対応 OS | 備考 |
| ---------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| MetaMask         | MacOS Chrome 拡張機能<br />Windows Chrome 拡張機能<br />iOS アプリ（iOS Safari 経由） |                                                                       |
| Bitget Wallet    | MacOS Chrome 拡張機能<br />iOS アプリ（iOS Safari 経由）<br />iOS アプリ内ブラウザ       | [Bitget Wallet（iOS アプリ）](supported-wallets/bitget-wallet-ios-app.md) |
| Coinbase Wallet  | MacOS Chrome 拡張機能                                                            |                                                                       |
| OKX Wallet       | MacOS Chrome 拡張機能                                                            |                                                                       |
| Rabby Wallet     | MacOS Chrome 拡張機能                                                            | [Rabby Wallet](supported-wallets/rabby-wallet.md)                     |
| Trust Wallet     | MacOS Chrome 拡張機能<br />iOS アプリ（iOS Safari 経由）                               | [Trust Wallet](supported-wallets/trust-wallet.md)                     |
| Phantom Wallet   | MacOS Chrome 拡張機能                                                            | [Phantom Wallet](supported-wallets/phantom-wallet.md)                 |

**注意**：「iOS アプリ（iOS Safari 経由）」とは、Safari ブラウザからウォレットアプリに接続する方式を指します。

### 非対応ウォレット

- **INTMAX Wallet** — 他のウォレットアプリで生成されるアドレスとは異なるアドレスが生成されます
- **Safe Wallet** — 署名フォーマットの非互換により、署名検証に失敗します
- **ハードウェアウォレット** — 接続のたびに異なる INTMAX アドレスが生成される場合があります
- **Coinbase Wallet（iOS Safari）** — 接続できません
- **Rabby Wallet（iOS Safari）** — 接続できません
- **OKX Wallet（iOS Safari）** — ETH の Deposit ができません

## ウォレットを Sepolia テストネットワークに接続する

テストネット環境を利用するには、ウォレットを **Sepolia ネットワーク** に接続する必要があります。
一部のウォレットアプリでは、デフォルトのネットワーク一覧に Sepolia が含まれていません。その場合は、手動で Sepolia ネットワークを追加する必要があります。
カスタムネットワークの追加方法については、お使いのウォレットの公式ドキュメントまたはアプリ内の案内をご参照ください。テストネット関連の操作を行う前に、Sepolia ネットワークが追加・選択されていることを確認してください。

## 補足事項

1. モバイル端末でモバイルウォレットを使用する場合は、各ウォレットアプリ内の**アプリ内ブラウザ**から dApp にアクセスすることを強く推奨します。Chrome や Brave などの外部モバイルブラウザ経由で dApp に接続すると、ウォレットの動作が不安定になる場合があります。これにより、dApp との通信やトランザクションの署名に問題が発生する可能性があります。アプリ内ブラウザを使用することで、ウォレットと dApp が同一のセキュアな環境内で動作し、接続の問題を大幅に軽減できます。
2. ウォレットアプリ A で作成した秘密鍵を別のウォレットアプリ B にインポートした場合、ウォレット A とウォレット B で接続した際に同じ INTMAX アドレスが生成される**保証はありません**。
   接続には常に同じウォレットアプリを使用してください。
