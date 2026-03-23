---
icon: rocket
description: RainbowKit/Wagmi プロジェクトへの INTMAX WalletSDK プラグイン統合手順
---

# RainbowKit 向けクイックスタートガイド

`RainbowKit`/`Wagmi` SDK は、ブロックチェーンベースのアプリケーションをさまざまなウォレットプロバイダーと統合するための強力なツールキットです。INTMAX WalletSDK は、ブロックチェーン Web ウォレットへの接続、ウォレット接続の管理、ブロックチェーンネットワークとのインタラクションのプロセスを簡素化します。幅広いブロックチェーンネットワークとウォレットプロバイダーをサポートし、柔軟で使いやすい設計です。

`RainbowKit` のセットアップに INTMAX WalletSDK を統合するためのクイックスタートガイドです。これにより、`RainbowKit` から INTMAX WalletSDK 対応のウォレット接続が可能になります。

## インストール

**前提条件**: Node.js 18.0+

プロジェクトで INTMAX WalletSDK の `RainbowKit`/`Wagmi` プラグインを使用するには、npm または yarn でインストールします：

{% tabs %}
  {% tab title="npm" %}
```sh
npm install intmax-walletsdk @rainbow-me/rainbowkit wagmi --save
```
  {% endtab %}
  {% tab title="yarn" %}
```sh
yarn add intmax-walletsdk @rainbow-me/rainbowkit wagmi
```
  {% endtab %}
{% endtabs %}

## 使い方

### SDK のインポート

インストール後、SDK から必要なコンポーネントをプロジェクトにインポートします。`intmaxwalletsdk` 関数とその他の必要なモジュールのインポート例を以下に示します。

```ts
import { intmaxwalletsdk } from "intmax-walletsdk/rainbowkit";
// ...
```

### ウォレットプロバイダーの追加

次に、アプリケーションにウォレットプロバイダーを追加します。以下の例では、`intmaxwalletsdk` 関数を使用してカスタムウォレットプロバイダーを追加する方法を示しています：

```ts
const additionalWallets = [
  intmaxwalletsdk({
    wallet: {
      url: "https://intmaxwallet-sdk-wallet.vercel.app/",
      name: "IntmaxWalletSDK Demo",
      iconUrl: "https://intmaxwallet-sdk-wallet.vercel.app/vite.svg",
    },
    metadata: {
      name: "Rainbow-Kit Demo",
      description: "Rainbow-Kit Demo",
      icons: ["https://intmaxwallet-sdk-wallet.vercel.app/vite.svg"],
    },
  }),
];
```

### React との統合

最後に、アプリケーションコンポーネントを `RainbowKitProvider` と `WagmiConfig` でラップして、SDK を React アプリケーションに統合します：

```ts
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { RainbowKitProvider, connectorsForWallets } from "@rainbow-me/rainbowkit";
import { WagmiConfig, createConfig } from "wagmi";
​
const connectors = connectorsForWallets([
 {
    groupName: "IntmaxWalletSDK",
    wallets: additionalWallets,
 },
]);
​
const wagmiConfig = createConfig({
 autoConnect: true,
 connectors,
 publicClient,
});
​
const root = document.getElementById("root");
if (!root) throw new Error("No root element found");
​
ReactDOM.createRoot(root).render(
 <React.StrictMode>
    <WagmiConfig config={wagmiConfig}>
      <RainbowKitProvider chains={chains}>
        <App />
      </RainbowKitProvider>
    </WagmiConfig>
 </React.StrictMode>,
);
```

その他のメソッドやイベントについては [API リファレンス](developers-hub/intmax-wallet-sdk/api-reference/index.md)セクションを参照してください。
エンドツーエンドのコード例は [wallet example](https://github.com/InternetMaximalism/intmax-walletsdk/tree/main/examples/rainbowkit) GitHub リポジトリで確認できます。
