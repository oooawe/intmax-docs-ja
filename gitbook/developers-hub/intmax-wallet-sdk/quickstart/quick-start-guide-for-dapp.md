---
icon: rocket
description: React dApp への INTMAX WalletSDK 統合手順と主要メソッドの使用例
---

# dApp 向けクイックスタートガイド

React dApp に INTMAX WalletSDK をセットアップするためのクイックスタートガイドです。Next.js にも軽微な調整で対応できます。この dApp から、対応する任意の Web ウォレットに接続できるようになります。

## インストール

**前提条件**: Node.js 18.0+

dApp の接続には intmax-walletsdk をインストールし、dApp 固有の機能のために `intmax-walletsdk/dapp` をインポートする必要があります。

{% tabs %}
  {% tab title="npm" %}
```sh
npm install intmax-walletsdk
```
  {% endtab %}
  {% tab title="yarn" %}
```sh
yarn add intmax-walletsdk
```
  {% endtab %}
  {% tab title="pnpm" %}
```sh
pnpm add intmax-walletsdk
```
  {% endtab %}
{% endtabs %}

## 使い方

### `intmaxDappClient` のインポートとクライアント作成

`intmax-walletsdk/dapp` から `intmaxDappClient` をインポートして設定します。設定には、名前、ウォレット URL、dApp のメタデータ、プロバイダーの指定が必要です。

```ts
// App.tsx

import { ethereumProvider, intmaxDappClient } from "intmax-walletsdk/dapp";

const createsdk = (walletUrl: string) => {
  return intmaxDappClient({
    wallet: { url: walletUrl, name: "DEMO Wallet", window: { mode: "iframe" } },
    metadata: DAPP_METADATA,
    providers: { eip155: ethereumProvider() },
  });
};
```

メタデータには、dApp の名前、説明、アイコンを含めます。

#### 使用例

```ts
const DEFAULT_WALLET_URL = "https://wallet.intmax.io";
const DAPP_METADATA = {
  name: "Dapp Example",
  description: "This is a simple example of how to use the dapp client.",
  icons: [DEFAULT_DAPP_ICON_URL],
};
```

この例では、SDK のカスタム Web3 プロバイダーである `ethereumProvider` を使用するよう `intmaxDappClient` を設定しています。その他の設定オプションについては ethereumProvider のドキュメントを参照してください。
クライアントの設定後、任意のウォレット URL を渡して使用できます。

```ts
const sdk = createsdk(DEFAULT_WALLET_URL);
```

### ウォレット接続の初期化

ウォレットアカウントに接続するには、プロバイダーを初期化し、`eth_requestAccounts` メソッドで dApp とウォレット間のペアリングプロセスを開始します。`eth_accounts` はウォレット内の利用可能なアカウントを取得します。

#### 使用例

```ts
const handleConnect = async () => {
  const ethereum = await sdk.provider("eip155");
  await ethereum.request({ method: "eth_requestAccounts", params: [] });
  const accounts = (await ethereum.request({ method: "eth_accounts", params: [] })) as string[];
};
```

### トランザクションの署名

トランザクション署名の関数を設定するには、`eth_sign` メソッドを呼び出します。これにより、ウォレットのポップアップが表示され、後で使用するトランザクションに署名できます。このメソッドには、ウォレットアドレスと署名対象のメッセージをパラメータとして渡す必要があります。

#### 使用例

```ts
const handleSignMessage = async () => {
  if (accounts.length === 0) await handleConnect();

  const ethereum = await sdk.provider("eip155");
  const _accounts = (await ethereum.request({ method: "eth_accounts", params: [] })) as string[];
  const result = await ethereum.request({
    method: "eth_sign",
    params: [_accounts[0], "Hello Webmax"],
  });
};
```

### トランザクションの送信

トランザクションを送信するには、`eth_sendTransaction` メソッドを使用します。このメソッドには、送信先アドレス（to）、送信元アドレス（from）、送金額（value）を渡す必要があります。その他のオプションパラメータについては [API リファレンス](/gitbook/developers-hub/intmax-wallet-sdk/api-reference/index.md)を参照してください。

#### 使用例

```ts
const handleSendTransaction = async () => {
  if (accounts.length === 0) await handleConnect();

  const ethereum = await sdk.provider("eip155");
  const _accounts = (await ethereum.request({ method: "eth_accounts", params: [] })) as string[];
  const result = await ethereum.request({
    method: "eth_sendTransaction",
    params: [{ from: _accounts[0], to: _accounts[0], value: "0x0" }],
  });
};
```

### ウォレットチェーンの切り替え

INTMAX WalletSDK では、接続中のウォレットで異なるチェーンへの切り替えも可能です。`wallet_switchEthereumChain` メソッドを呼び出し、切り替え先のチェーン ID をパラメータとして渡します。これにより、指定したネットワークへの切り替え確認がユーザーに表示されます。

#### 使用例

```ts
await ethereum.request({ method: "wallet_switchEthereumChain", params: [{ chainId: "0x89" }] });
```

### 型付きデータの署名

intmax-walletsdk では、`eth_signTypedData_v4` メソッドを使用して型付きデータ（Typed Data）の署名を処理できます。このメソッドは EIP-712（型付き構造化データ署名の標準）の一部です。`eth_signTypedData_v4` には型付きデータをパラメータとして渡す必要があります。

#### 使用例

```ts
const handleSignTypedData = async () => {
  if (accounts.length === 0) await handleConnect();

  const typedData = {
    types: {
      EIP712Domain: [
        { name: "name", type: "string" },
        { name: "version", type: "string" },
      ],
      Person: [
        { name: "name", type: "string" },
        { name: "age", type: "uint256" },
      ],
    },
    domain: { name: "Webmax Dapp Example", version: "1" },
    primaryType: "Person",
    message: { name: "Bob", age: 25 },
  };

  const ethereum = await sdk.provider("eip155");
  const _accounts = (await ethereum.request({ method: "eth_accounts", params: [] })) as string[];
  const result = await ethereum.request({
    method: "eth_signTypedData_v4",
    params: [_accounts[0], JSON.stringify(typedData)],
  });

  setResult(result as string);
};
```

WalletNext がサポートするその他のメソッドについては [API リファレンス](/gitbook/developers-hub/intmax-wallet-sdk/api-reference/index.md)を参照してください。
エンドツーエンドのコード例は [dapp example](https://github.com/InternetMaximalism/walletnext/tree/main/examples/dapp) GitHub リポジトリで確認できます。
