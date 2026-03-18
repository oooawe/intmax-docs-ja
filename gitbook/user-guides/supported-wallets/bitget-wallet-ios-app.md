---
icon: wallet
description: Bitget Wallet iOS アプリでの Sepolia ネットワーク追加と接続手順
---

# Bitget Wallet（iOS アプリ）

Bitget Wallet は Ethereum メインネットでは問題なく動作しますが、Ethereum Sepolia テストネットワークでの使用にはいくつかの制限があります。

iOS 版アプリでは、インポートした秘密鍵（Private Key）を使って Ethereum Sepolia ネットワークを追加することはできません。ニーモニックフレーズで作成したウォレット、またはキーレスウォレットでのみ使用可能です。

また、WalletConnect の接続に失敗した場合は、WalletConnect のポップアップを一度閉じてから再度開き、QR コードをスキャンすることで次の画面に進めることがあります。

## Bitget Wallet で Sepolia ネットワークを追加するには

1. 画面右上に表示されているネットワーク名をタップ
2. 「Add Network」を選択

<figure>
  <img src="assets/user-guides/bitget_wallet_10.webp" alt="Network Name" />
  <img src="assets/user-guides/bitget_wallet_20.webp" alt="Add Network" />
</figure>

3. 検索バーに「Sepolia」と入力し、候補一覧から「Ethereum Sepolia」を選択
4. 「Adding network is not supported yet」というメッセージが表示された場合、現在使用しているアドレスでは Sepolia ネットワークを追加できません。
   その場合は、新しいウォレットの作成、またはニーモニックフレーズのインポートもしくはキーレスウォレットによるウォレットの復元を行ってください。

<figure>
  <img src="assets/user-guides/bitget_wallet_30.webp" alt="Ethereum Sepolia" />
  <img src="assets/user-guides/bitget_wallet_40.webp" alt="Adding Network Not Supported" />
</figure>

5. 「WalletConnect connection failed」というメッセージが表示された場合は、Web サイト上の WalletConnect ポップアップを閉じてから再度開いてください。
   その後、QR コードを再度スキャンして接続をお試しください。

<figure class="no-max-height">
  <img src="assets/user-guides/bitget_wallet_50.webp" width="300" alt="WalletConnect Connection Failed" />
  <br />
  <img src="assets/user-guides/bitget_wallet_60.webp" width="300" alt="WalletConnect QR Code" />
</figure>
