---
icon: book-open
description: ウォレットにトークンの追跡を要求するメソッド
---

# Watch Wallet Asset

指定されたトークンをウォレットで追跡するようユーザーに要求するメソッドです。トークンが正常に追加されたかどうかを示すブーリアン値を返します。追加されたトークンは、一元管理されたレジストリなどの従来の方法で追加されたトークンと区別がつきません。

## リクエストパラメータ

| パラメータ | 型 | 必須 | 説明 |
| ------------------ | ------- | -------- | ------------------------------------------------------------------------------- |
| `type`             | String  | Yes      | ERC-20、ERC-721、ERC-1155 トークンをサポート |
| `options`          | Object  | Yes      | 以下のパラメータを含むオブジェクト |
| `options.address`  | String  | Yes      | トークンコントラクトのアドレス |
| `options.symbol`   | String  | No       | ティッカーシンボルまたは略称（最大 11 文字、ERC-20 ではオプション） |
| `options.decimals` | Integer | No       | トークンの小数点以下の桁数（ERC-20 ではオプション） |
| `options.image`    | String  | No       | トークンロゴの URL 文字列（ERC-20 ではオプション） |
| `options.tokenId`  | String  | No       | NFT の一意識別子（ERC-721 および ERC-1155 では必須） |

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| --------- | ------- | ------------------------------------------------- |
| `DATA`    | Boolean | トークンが追加された場合は `true`、それ以外は `false` |

## リクエスト例

```typescript
ethereum.request({
  method: "wallet_watchAsset",
  params: {
    type: "ERC20",
    options: {
      address: "0xb60e8dd61c5d32be8058aabeb970870f07233155",
      symbol: "FOO",
      decimals: 18,
      image: "https://foo.io/token-image.svg",
    },
  },
});
```

## レスポンス例

```json
true
```
