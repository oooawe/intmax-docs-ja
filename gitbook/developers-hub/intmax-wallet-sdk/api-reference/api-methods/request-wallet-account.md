---
icon: book-open
description: ユーザーの Ethereum アドレスを要求するメソッド
---

# Request Wallet Account

ユーザーに識別用の Ethereum アドレスの提供を要求するメソッドです。

## リクエストパラメータ

なし

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| ------------- | --------------- | ------------------------- |
| `AddressList` | Array\[string\] | ウォレットアドレスの配列 |

## リクエスト例

```typescript
// For wallet side example
const intmax = intmaxWalletClient();
intmax.on("eip155/eth_requestAccounts", (c) => {
  return c.success({
    accounts: ["0x1234..."],
  });
});

// For dapp side example
const ethereum = await webmax.provider("eip155");
await ethereum.request({ method: "eth_requestAccounts", params: [] });
```

## レスポンス例

```json
"0xa22392123a1095f75e62abc7dea7e0e1e5142d5f"
```
