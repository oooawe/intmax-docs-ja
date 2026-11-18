---
icon: book-open
description: ウォレットへの接続を要求するメソッド
---

# INTMAX Connect

ユーザーにウォレットとの接続を要求するメソッドです。

## リクエストパラメータ

なし

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| --------------------- | ---- | ----------- |
| `IntmaxConnectResult` |      |             |

## リクエスト例

```typescript
Request Example
const intmax = intmaxWalletClient();
webmax.on("intmax/intmax_connect", async (c) => {
if (isConnected(c, connections)) {
	return c.success({
		supportedNamespaces: ["eip155", "intmax"],
		supportedChains: supportedChains,
		accounts: { eip155: ethereumAccounts },
	});
}
```

## レスポンス例

```typescript
type IntmaxConnectResult = {
  supportedNamespaces: Namespace[];
  supportedChains: ChainedNamespace[];
  accounts: {
    eip155: EthereumAddress[];
  };
};
```
