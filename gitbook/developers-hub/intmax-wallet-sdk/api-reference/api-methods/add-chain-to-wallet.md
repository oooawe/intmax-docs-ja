---
icon: book-open
description: ウォレットに新しいチェーンを追加するメソッド
---

# Add Chain To Wallet

指定されたチェーンをウォレットアプリケーションに追加する確認をユーザーに求めるメソッドです。呼び出し側はチェーン ID とチェーンのメタデータを指定する必要があります。

## リクエストパラメータ

### Params\[0\]

| パラメータ | 型 | 必須 | 説明 |
| ------------------------- | --------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| `chainId`                 | String          | Yes      | `0x` プレフィックス付きの16進数文字列 |
| `chainName`               | String          | Yes      | チェーンの人間が読める名称 |
| `rpcUrls`                 | Array\[string\] | Yes      | チェーンとの通信に使用できる RPC エンドポイントの URL（1 つ以上） |
| `iconUrls`                | Array\[string\] | No       | チェーンを視覚的に識別するための適切なサイズの画像 URL（1 つ以上） |
| `nativeCurrency`          | Object          | Yes      | `name`、`symbol`、`decimals` フィールドを使用してチェーンのネイティブ通貨を記述 |
| `nativeCurrency.decimals` | Integer         | Yes      | 非負の整数 |
| `nativeCurrency.symbol`   | String          | Yes      | 人間が読める通貨シンボル |
| `nativeCurrency.name`     | String          | No       | 人間が読める通貨名 |
| `blockExplorerUrls`       | Array\[string\] | No       | チェーンのブロックエクスプローラーサイトの URL（1 つ以上） |

## レスポンスパラメータ

| パラメータ | 型 | 説明 |
| --------- | ---- | ---------------------------------------------------------------------------------------- |
| `null`    |      | リクエストが成功した場合は `null` を返し、失敗した場合はエラーを返す |

## リクエスト例

```typescript
ethereum.request({
  "method": "wallet_addEthereumChain",
  "params": [
    {
      "chainId": "0x64",
      "chainName": "Gnosis",
      "rpcUrls": [
        "https://rpc.ankr.com/gnosis"
      ],
      "iconUrls": [
        "https://xdaichain.com/fake/example/url/xdai.svg",
        "https://xdaichain.com/fake/example/url/xdai.png"
      ],
      "nativeCurrency": {
        "decimals": 18
        "symbol": "xDAI",
        "name": "xDAI",
      },
      "blockExplorerUrls": [
        "https://blockscout.com/poa/xdai/"
      ]
    }
  ]
})
```

## レスポンス例

```json
null
```
