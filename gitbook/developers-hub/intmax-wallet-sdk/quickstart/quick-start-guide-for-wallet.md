---
icon: rocket
description: Web ウォレットへの INTMAX WalletSDK 統合手順とイベントハンドリングの基本
---

# ウォレット向けクイックスタートガイド

Web ウォレットに intmax-walletsdk をセットアップするためのクイックスタートガイドです。これにより、Wallet Next 対応の dApp からウォレットへの接続が可能になります。

## インストール

**前提条件**: Node.js 18.0+

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

ウォレットクライアント `intmaxWalletClient` をインポートして初期化します。

```ts
import { intmaxWalletClient } from "intmax-walletsdk/wallet";

const sdk = intmaxWalletClient();
```

`intmaxWalletClient` は、ウォレットと dApp 間のインタラクションを円滑にするためのメソッド群を提供します。これらのメソッドは、イベントリスナーの設定、準備完了の通知、リソースのクリーンアップに不可欠です。利用可能なメソッドは以下の通りです：

### `.on(path, cb)`

特定のイベント発生時に実行されるコールバック関数を登録します。`.on()` メソッドで特定のイベントをリッスンし、適切に処理します。

**パラメータ：**

- `path`（string）：イベントパス。通常はネームスペースとメソッド名の組み合わせです（例：`"eip155/eth_requestAccounts"`、`"intmax/intmax_ready"` など）。
- `cb`（function）：実行されるコールバック関数。`context` オブジェクトを受け取り、`AbstractResponse` またはそれを解決する Promise を返す必要があります。イベントパスと期待される戻り値については API リファレンスを参照してください。

#### 使用例

```ts
// To handle account request events
sdk.on("eip155/eth_requestAccounts", (context) => {
  return context.success({
    accounts: ["0x1234..."],
  });
});

// To connect dapp to wallet
sdk.on("intmax/intmax_connect", async (c) => {
	if (isConnected(c, connections)) {
		return c.success({
			supportedNamespaces: ["eip155", "intmax"],
			supportedChains: supportedChains,
			accounts: { eip155: ethereumAccounts },
		});
	}
}

// To sign transaction
sdk.on("eip155/eth_sign", async (c) => {
	const [address, data] = c.req.params;
	try {
		const signature = await signMessage(address, data, c.req.metadata);
		return c.success(signature);
	} catch {
		return c.failure("", { code: 4001 });
	}
});
```

### `.ready()`

ウォレットがリクエストを処理する準備ができたことを通知します。このメソッドは親ウィンドウに `intmax_ready` メッセージを送信します。

**パラメータ：** なし

#### 使用例

```ts
sdk.ready();
```

### `.destruct()`

イベントリスナーの削除とメッセージディスパッチの停止により、リソースをクリーンアップします。ウォレットが不要になった場合や閉じる前に、`.destruct()` メソッドを呼び出してリソースを解放してください。

**パラメータ：** なし

#### 使用例

```ts
sdk.destruct();
```

これらのメソッドを活用することで、`intmaxWalletClient` をウォレットに効果的に統合し、ウォレットと dApp 間のシームレスな通信・インタラクションを実現できます。

その他のメソッドやイベントについては [API リファレンス](../api-reference/)セクションを参照してください。
エンドツーエンドのコード例は [wallet example](https://github.com/InternetMaximalism/intmax-walletsdk/tree/main/examples/wallet) GitHub リポジトリで確認できます。
