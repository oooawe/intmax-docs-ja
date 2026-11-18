---
icon: book-open
description: Ethereum 固有の署名を生成するメソッド
---

# Sign Message

Ethereum 固有の署名（Signature）を計算するメソッドです。メッセージにプレフィックスを付加することで、Ethereum 固有の署名として識別できるようにします。これにより、悪意のある dApp が任意のデータ（例：トランザクション）に署名させ、その署名を利用してユーザーになりすますことを防止します。

## リクエストパラメータ

### Params\[0\]

| パラメータ | 型 | 必須 | 説明 |
| --------- | ------ | -------- | ------------------ |
| `DATA`    | String | Yes      | アドレス（20 Bytes） |

### Params\[1\]

| パラメータ | 型 | 必須 | 説明 |
| --------- | ------ | -------- | ------------------------- |
| `DATA`    | String | Yes      | 署名するメッセージ（N Bytes） |

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| --------- | ------ | ----------- |
| `DATA`    | String | 署名 |

## リクエスト例

```typescript
ethereum.request({
  method: "eth_sign",
  params: ["0x7a2054d370f93ec7d8a08e065121118dc8f4bf29", "Hello Webmax"],
});
```

## レスポンス例

```json
"0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
```
