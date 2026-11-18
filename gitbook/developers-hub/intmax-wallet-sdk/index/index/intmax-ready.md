---
icon: book-open
description: ウォレット初期化完了を dApp に通知するメソッド
---

# INTMAX Ready

ウォレットの初期化が完了し、dApp との通信が可能になったことを通知するメソッドです。

## リクエストパラメータ

なし

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| ------------------- | ---- | ----------- |
| `IntmaxReadyResult` |      |             |

## リクエスト例

```typescript
const sdk = intmaxWalletClient();

sdk.on("intmax/intmax_ready", (c) => {
  return c.success({
    supportedNamespaces: ["eip155", "intmax"],
    supportedChains: supportedChains,
  });
});
```

### レスポンス例

```typescript
export type WebmaxReadyResult = {
  supportedNamespaces: Namespace[];
  supportedChains: ChainedNamespace[];
};
```
