---
icon: plug
description: intmaxDappClient の Provider メソッドと EthereumProvider の設定方法
---

# Providers

## `.provider` メソッド

`.provider` メソッドは `INTMAX WalletSDK` の `intmaxDappClient` における重要な機能であり、異なるブロックチェーンプロバイダーとの直接的なインタラクションを可能にします。カスタムプロバイダーの使用をサポートしており、SDK が直接対応していないブロックチェーンネットワークとの統合も柔軟に行えます。その仕組みと効果的な使い方を説明します。

### メソッドシグネチャ

```typescript
provider<NS extends Schema[number]["namespace"]>(namespace: ChainedNamespace<NS> | NS)
```

- `namespace`：インタラクション対象のブロックチェーンプロバイダーの Namespace を表す文字列です。単純な Namespace（例：`"eip155"`）またはチェーン付き Namespace（例：`"eip155:1"`、番号はチェーン ID）を指定できます。

### 使用例

```typescript
const ethereum = await sdk.provider("eip155");
// or for a specific chain ID
const ethereumChainSpecific = await sdk.provider("eip155:1");
```

この例では、`client.provider("eip155")` が Ethereum プロバイダーのインスタンスを返し、Ethereum ブロックチェーンとのインタラクションに使用できます。Ethereum Mainnet（チェーン ID 1）など特定のチェーンとインタラクションする必要がある場合は、Namespace の一部としてチェーン ID を指定します（例：`"eip155:1"`）。

## EthereumProvider

`ethereumProvider` は、EIP-1193 標準に準拠した Ethereum プロバイダーインスタンスを作成するファクトリ関数です。このプロバイダーにより、一連のメソッドを通じて Ethereum ブロックチェーンとインタラクションできます。dApp からのアカウント情報のリクエスト、チェーンの切り替え、トランザクション署名などの各種ブロックチェーン操作に加え、`eth_chainId`、`eth_accounts`、`wallet_switchEthereumChain` などの Ethereum JSON-RPC リクエストを処理できます。

### 設定

`ethereumProvider` はオプションの `EthereumProviderOptions` オブジェクトを受け取ります：

- `httpRpcUrls`：Ethereum ブロックチェーンとのインタラクション用に、チェーン ID と HTTP RPC URL のマッピングを指定します。ブロックチェーンとの直接的なインタラクションが必要な操作に必須です。

### 使用例

dApp で `ethereumProvider` を使用するには、まずプロバイダー設定で `intmaxDappClient` を初期化します：

```typescript
const client = intmaxDappClient({
  wallet: { url: walletUrl, name: "DEMO Wallet" },
  metadata: DAPP_METADATA,
  providers: {
    eip155: ethereumProvider({
      httpRpcUrls: {
        1: "https://mainnet.infura.io/v3",
        137: "https://rpc-mainnet.maticvigil.com",
      },
    }),
  },
});
```

この例では、Ethereum Mainnet（チェーン ID 1）と Matic Mainnet（チェーン ID 137）の HTTP RPC URL で `ethereumProvider` を設定しています。
