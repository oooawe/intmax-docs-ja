---
icon: book-open
description: ユーザーが所有するアカウントのアドレス一覧を取得するメソッド
---

# Get Accounts

ユーザーが所有するアカウントのアドレス一覧を返します。

## リクエストパラメータ

なし

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| ------------- | --------------- | ------------------------------ |
| `AddressList` | Array\[string\] | クライアントが所有するアドレス |

## リクエスト例

```typescript
await window.ethereum.request({
  method: "eth_accounts",
  params: [],
});
```

## レスポンス例

```json
"0xD88392123a1085c75e62eAc7dea7e0e1e5142d5f"
```
