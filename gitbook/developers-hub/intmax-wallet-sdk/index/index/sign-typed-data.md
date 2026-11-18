---
icon: book-open
description: 構造化データの署名を要求するメソッド
---

# Sign Typed Data

ユーザーに対して構造化された可読形式でデータメッセージを提示し、署名済みレスポンスを返すメソッドです。このメソッドを使用するには、ユーザーがアカウントへのアクセスを事前に許可している必要があります。先に `eth_requestAccounts` を呼び出してください。

## リクエストパラメータ

### Params\[0\]

| パラメータ | 型 | 必須 | 説明 |
| --------- | ------ | -------- | -------------------------------------------------------- |
| `DATA`    | String | Yes      | 署名を要求するアカウントのアドレス（20 Bytes） |

### Params\[1\]

| パラメータ | 型 | 必須 | 説明 |
| -------------------- | ------ | -------- | ------------------------------------------------------------------------------ |
| `types`              | Object | Yes      | 署名するメッセージ（N Bytes） |
| `types.EIP712Domain` | Array  | Yes      | 以下のドメインセパレータ値を 1 つ以上指定する配列 |
| `domain`             | Object | Yes      | `EIP712Domain` 型で指定されたドメインセパレータ値を含むオブジェクト |
| `primaryType`        | String | Yes      |                                                                                |
| `message`            | Object | Yes      | ユーザーに署名を提案するメッセージ |

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
|-----------|----------|-------------|
| `DATA` | String | 署名 |

## リクエスト例

```typescript
await window.ethereum.request({
  "method": "eth_signTypedData_v4",
  "params": [
    "0x0000000000000000000000000000000000000000",
    {
      "types": {
        "EIP712Domain": [
          {
            "name": "name",
            "type": "string"
          },
          {
            "name": "version",
            "type": "string"
          },
          {
            "name": "chainId",
            "type": "uint256"
          },
          {
            "name": "verifyingContract",
            "type": "address"
          }
        ],
        "Person": [
          {
            "name": "name",
            "type": "string"
          },
          {
            "name": "wallet",
            "type": "address"
          }
        ],
        "Mail": [
          {
            "name": "from",
            "type": "Person"
          },
          {
            "name": "to",
            "type": "Person"
          },
          {
            "name": "contents",
            "type": "string"
          }
        ]
      },
      "primaryType": "Person",
      "domain": {
        "name": "Webmax Dapp Example",
        "version": "1",
        "chainId": 1,
        "verifyingContract": "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC"
      },
      "message": {
        { name: "Bob", age: 25 }
      }
    }
  ]
});
```

## レスポンス例

```json
"0x4355c47d63924e8a72e509b65029052eb6c299d53a04e167c5775fd466751c9d07299936d304c153f6443dfa05f40ff007d72911b6f72307f996231605b915621c"
```
