---
icon: book-open
description: 現在のネットワークのチェーン ID を取得するメソッド
---

# Get Chain ID

現在のネットワークのチェーン ID を返します。

## リクエストパラメータ

なし

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| --------- | ------ | --------------------------------------------------- |
| `chainId` | String | `0x` プレフィックス付きの16進数文字列 |

## リクエスト例

```typescript
await window.ethereum.request({
  method: "eth_chainId",
  params: [],
});
```

## レスポンス例

```json
"0x1"
```
