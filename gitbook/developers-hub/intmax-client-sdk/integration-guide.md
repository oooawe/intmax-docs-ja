---
icon: puzzle-piece
description: intmax2-client-sdk と intmax2-server-sdk による INTMAX ネットワーク統合の実践ガイド
---

# 統合ガイド

本ドキュメントでは、**intmax2-client-sdk** と **intmax2-server-sdk** を使用した INTMAX ネットワークとの統合について、実際のコード例を交えた実践ガイドを提供します。

各セクションでは、SDK のインストールとクライアント初期化からログイン、トークンの Deposit・Withdrawal、トランザクション履歴の取得、署名検証まで、主要なステップを網羅しています。これにより、INTMAX のプライバシー保護機能を活用したアプリケーションを容易に構築できます。

例を順に追うことで、実際の開発シナリオにおける INTMAX SDK の効果的な使い方を直感的に理解できます。

**関連ドキュメント**

包括的なドキュメントとリソースについてはこちらを参照してください：

[Client SDK 概要を見る](developers-hub/intmax-client-sdk/overview.md)

# intmax2-client-sdk をはじめる

このガイドでは、`intmax2-client-sdk` を使用して INTMAX ネットワークとやり取りするためのステップバイステップの例を示します。この SDK は主に**フロントエンドアプリケーション**での使用を想定しています。クライアントのセットアップ、ログイン、Deposit・Withdrawal・トランザクション履歴の管理まですべてを網羅しています。

完全なサンプルコードは GitHub リポジトリを参照してください：

[Vite サンプルを GitHub で見る](https://github.com/InternetMaximalism/intmax2-client-sdk/tree/main/examples/vite)

## 例: Vite + React + TypeScript

```bash
npm create vite@latest my-app # Select React and TypeScript
cd my-app
```

### インストール

任意のパッケージマネージャーで SDK をインストールします：

```bash
# npm
npm i intmax2-client-sdk

# yarn
yarn add intmax2-client-sdk

# pnpm
pnpm i intmax2-client-sdk
```

### ステート変数

INTMAX クライアントの初期化、認証、破棄を行う軽量な React フックです。まず、必要なステート変数を定義します。

```tsx
import { useState } from "react";

import { IntMaxClient } from "intmax2-client-sdk";

export const useIntMaxClient = () => {
  const [client, setClient] = useState<IntMaxClient | null>(null);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  return { client, isLoggedIn, loading, error };
};
```

| **変数** | **用途** |
| ------------ | ------------------------------------------------ |
| client       | インスタンス化された **IntMaxClient** または null を保持 |
| isLoggedIn   | ユーザーのログインが成功すると true |
| loading      | 非同期処理の実行中は true |
| error        | 最後のエラーメッセージ。エラーがなければ null |

### INTMAX Client の初期化

新しい Testnet クライアントを作成し、ステートに保存します。

```tsx
import { useCallback } from "react";
import { IntMaxClient } from "intmax2-client-sdk";

const initializeClient = useCallback(async () => {
  try {
    setLoading(true);
    setError(null);

    const newClient = await IntMaxClient.init({
      environment: "testnet",
    });

    setClient(newClient);
  } catch (err) {
    const errorMessage = err instanceof Error ? err.message : "Failed to initialize client";
    setError(errorMessage);
    console.error("IntMax Client initialization failed:", err);
  } finally {
    setLoading(false);
  }
}, []);
```

### ログイン / アカウントリカバリー

ユーザーの INTMAX アカウントを作成（またはリカバリー）し、isLoggedIn を true に切り替えます。

```tsx
const login = useCallback(async () => {
  if (!client) {
    setError("Client not initialized");
    return;
  }

  try {
    setLoading(true);
    setError(null);
    await client.login();
    setIsLoggedIn(true);
  } catch (err) {
    const errorMessage = err instanceof Error ? err.message : "Login failed";
    setError(errorMessage);
    console.error("Login failed:", err);
  } finally {
    setLoading(false);
  }
}, [client]);
```

### ログアウト / セッションリセット

ログイン状態をローカルでクリアし、サーバー上のセッションを無効化します。

```tsx
const logout = useCallback(async () => {
  if (!client) return;

  try {
    setLoading(true);
    await client.logout();
    setIsLoggedIn(false);
    setError(null);
  } catch (err) {
    const errorMessage = err instanceof Error ? err.message : "Logout failed";
    setError(errorMessage);
    console.error("Logout failed:", err);
  } finally {
    setLoading(false);
  }
}, [client]);
```

### まとめ

以下は完全なフックのコードです：

```tsx
// hooks/useIntMaxClient.tsx
import { useState, useCallback } from "react";
import { IntMaxClient } from "intmax2-client-sdk";

export const useIntMaxClient = () => {
  const [client, setClient] = useState<IntMaxClient | null>(null);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const initializeClient = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);

      const newClient = await IntMaxClient.init({
        environment: "testnet",
      });

      setClient(newClient);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : "Failed to initialize client";
      setError(errorMessage);
      console.error("IntMax Client initialization failed:", err);
    } finally {
      setLoading(false);
    }
  }, []);

  const login = useCallback(async () => {
    if (!client) {
      setError("Client not initialized");
      return;
    }

    try {
      setLoading(true);
      setError(null);
      await client.login();
      setIsLoggedIn(true);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : "Login failed";
      setError(errorMessage);
      console.error("Login failed:", err);
    } finally {
      setLoading(false);
    }
  }, [client]);

  const logout = useCallback(async () => {
    if (!client) return;

    try {
      setLoading(true);
      await client.logout();
      setIsLoggedIn(false);
      setError(null);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : "Logout failed";
      setError(errorMessage);
      console.error("Logout failed:", err);
    } finally {
      setLoading(false);
    }
  }, [client]);

  return {
    client,
    isLoggedIn,
    loading,
    error,
    initializeClient,
    login,
    logout,
  };
};
```

以下の React コンポーネントは、useIntMaxClient フックを使用してアプリケーション内で INTMAX クライアントを管理する方法を示しています。

```tsx
// App.tsx
import { useIntMaxClient } from "./hooks/useIntMaxClient";

function App() {
  const { client, isLoggedIn, loading, error, initializeClient, login, logout } = useIntMaxClient();

  if (loading) {
    return (
      <div>
        <p>Initializing IntMax2 Client...</p>
      </div>
    );
  }

  return (
    <div>
      {error && (
        <div>
          <p>Error: {error}</p>
        </div>
      )}

      {!client ? (
        <div>
          <h2>Welcome to INTMAX Network</h2>
          <p>Initialize the client to get started</p>
          <button onClick={initializeClient} disabled={loading}>
            {loading ? "Initializing..." : "Initialize Client"}
          </button>
        </div>
      ) : !isLoggedIn ? (
        <div>
          <h2>Login to Your Account</h2>
          <p>Connect your INTMAX account to continue</p>
          <button onClick={login} disabled={loading}>
            {loading ? "Connecting..." : "Connect Wallet"}
          </button>
        </div>
      ) : (
        <div>
          <div>
            <h2>Your INTMAX Account</h2>
            <p>
              Address: <code>{client.address}</code>
            </p>
          </div>
          <div>
            <button onClick={logout}>Logout</button>
          </div>
        </div>
      )}
    </div>
  );
}

export default App;
```

### 次のステップ

**intmax2-client-sdk** では、Deposit・Withdrawal・Transfer やトランザクション履歴の取得機能も提供しています。詳細は SDK のドキュメントを参照してください。

[Client SDK リファレンスを見る](developers-hub/intmax-client-sdk/overview.md)

## 例: Next.js + TypeScript

`create-next-app` を使用して Next.js アプリケーションのボイラープレートを生成します。

コマンド例：

```bash
npx create-next-app@latest my-app
```

`create-next-app` でプロジェクトを作成する際、**Turbopack** を無効にするオプションを選択してください。

```
✔ Would you like to use TypeScript? … No / Yes <- Select "Yes"
✔ Would you like to use ESLint? … No / Yes <- Select "Yes"
✔ Would you like to use Tailwind CSS? … No / Yes <- Select "Yes"
✔ Would you like your code inside a `src/` directory? … No / Yes <- Select "Yes"
✔ Would you like to use App Router? (recommended) … No / Yes <- Select "Yes"
? Would you like to use Turbopack for `next dev`? › No / Yes <- Select "No"
```

その他の設定は任意です。すべての設定が完了したら、プロジェクトディレクトリに移動します。

```bash
cd my-app
```

プロジェクトルートにある `next.config.ts` ファイルを以下のように変更してください：

```tsx
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  webpack: (config) => {
    config.experiments = {
      ...config.experiments,
      asyncWebAssembly: true,
    };
    config.module.rules.push({
      test: /\.wasm$/,
      type: "asset/resource",
    });
    return config;
  },
};

export default nextConfig;
```

### インストール

任意のパッケージマネージャーで SDK をインストールします：

```bash
# npm
npm i intmax2-client-sdk

# yarn
yarn add intmax2-client-sdk

# pnpm
pnpm i intmax2-client-sdk
```

### ステート変数

INTMAX クライアントの初期化、認証、破棄を行う軽量な React フックです。まず、必要なステート変数を定義します。

```tsx
import { useState } from "react";

import { IntMaxClient } from "intmax2-client-sdk";

export const useIntMaxClient = () => {
  const [client, setClient] = useState<IntMaxClient | null>(null);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  return { client, isLoggedIn, loading, error };
};
```

| **変数** | **用途** |
| ------------ | ------------------------------------------------ |
| client       | インスタンス化された **IntMaxClient** または null を保持 |
| isLoggedIn   | ユーザーのログインが成功すると true |
| loading      | 非同期処理の実行中は true |
| error        | 最後のエラーメッセージ。エラーがなければ null |

### INTMAX Client の初期化

新しい Testnet クライアントを作成し、ステートに保存します。

```tsx
import { useCallback } from "react";
import { IntMaxClient } from "intmax2-client-sdk";

const initializeClient = useCallback(async () => {
  try {
    setLoading(true);
    setError(null);

    const newClient = await IntMaxClient.init({
      environment: "testnet",
    });

    setClient(newClient);
  } catch (err) {
    const errorMessage = err instanceof Error ? err.message : "Failed to initialize client";
    setError(errorMessage);
    console.error("IntMax Client initialization failed:", err);
  } finally {
    setLoading(false);
  }
}, []);
```

### ログイン / アカウントリカバリー

ユーザーの INTMAX アカウントを作成（またはリカバリー）し、isLoggedIn を true に切り替えます。

```tsx
const login = useCallback(async () => {
  if (!client) {
    setError("Client not initialized");
    return;
  }

  try {
    setLoading(true);
    setError(null);
    await client.login();
    setIsLoggedIn(true);
  } catch (err) {
    const errorMessage = err instanceof Error ? err.message : "Login failed";
    setError(errorMessage);
    console.error("Login failed:", err);
  } finally {
    setLoading(false);
  }
}, [client]);
```

### ログアウト / セッションリセット

ログイン状態をローカルでクリアし、サーバー上のセッションを無効化します。

```tsx
const logout = useCallback(async () => {
  if (!client) return;

  try {
    setLoading(true);
    await client.logout();
    setIsLoggedIn(false);
    setError(null);
  } catch (err) {
    const errorMessage = err instanceof Error ? err.message : "Logout failed";
    setError(errorMessage);
    console.error("Logout failed:", err);
  } finally {
    setLoading(false);
  }
}, [client]);
```

### まとめ

以下は完全なフックのコードです：

```tsx
// src/hooks/useIntMaxClient.tsx
import { useState, useCallback } from "react";
import { IntMaxClient } from "intmax2-client-sdk";

export const useIntMaxClient = () => {
  const [client, setClient] = useState<IntMaxClient | null>(null);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const initializeClient = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);

      const newClient = await IntMaxClient.init({
        environment: "testnet",
      });

      setClient(newClient);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : "Failed to initialize client";
      setError(errorMessage);
      console.error("IntMax Client initialization failed:", err);
    } finally {
      setLoading(false);
    }
  }, []);

  const login = useCallback(async () => {
    if (!client) {
      setError("Client not initialized");
      return;
    }

    try {
      setLoading(true);
      setError(null);
      await client.login();
      setIsLoggedIn(true);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : "Login failed";
      setError(errorMessage);
      console.error("Login failed:", err);
    } finally {
      setLoading(false);
    }
  }, [client]);

  const logout = useCallback(async () => {
    if (!client) return;

    try {
      setLoading(true);
      await client.logout();
      setIsLoggedIn(false);
      setError(null);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : "Logout failed";
      setError(errorMessage);
      console.error("Logout failed:", err);
    } finally {
      setLoading(false);
    }
  }, [client]);

  return {
    client,
    isLoggedIn,
    loading,
    error,
    initializeClient,
    login,
    logout,
  };
};
```

以下の React コンポーネントは、useIntMaxClient フックを使用してアプリケーション内で INTMAX クライアントを管理する方法を示しています。

```tsx
// src/app/page.tsx
import { useIntMaxClient } from '../hooks/useIntMaxClient'

default export function Home() {
  const {
    client,
    isLoggedIn,
    loading,
    error,
    initializeClient,
    login,
    logout
  } = useIntMaxClient()

  if (loading) {
    return (
      <div>
        <p>Initializing IntMax2 Client...</p>
      </div>
    )
  }

  return (
   <div>
      {error && (
        <div>
          <p>Error: {error}</p>
        </div>
      )}

      {!client ? (
        <div>
          <h2>Welcome to INTMAX Network</h2>
          <p>Initialize the client to get started</p>
          <button
            onClick={initializeClient}
            disabled={loading}
          >
            {loading ? 'Initializing...' : 'Initialize Client'}
          </button>
        </div>
      ) : !isLoggedIn ? (
        <div>
          <h2>Login to Your Account</h2>
          <p>Connect your INTMAX account to continue</p>
          <button
         onClick={login}
         disabled={loading}
       >
         {loading ? 'Connecting...' : 'Connect Wallet'}
       </button>
        </div>
      ) : (
        <div>
          <div>
            <h2>Your INTMAX Account</h2>
            <p>
              Address: <code>{client.address}</code>
            </p>
          </div>
          <div>
        <button
          onClick={logout}
        >
          Logout
        </button>
          </div>
        </div>
      )}
    </div>
  )
}
```

### 次のステップ

**intmax2-client-sdk** では、Deposit・Withdrawal・Transfer やトランザクション履歴の取得機能も提供しています。詳細は SDK のドキュメントを参照してください。

[INTMAX Client SDK ドキュメント](developers-hub/intmax-client-sdk/api-reference.md)

# intmax2-server-sdk の使い方

このガイドでは、`intmax2-server-sdk` を使用して INTMAX ネットワークとやり取りするためのステップバイステップの例を示します。この SDK は主に**サーバーサイドアプリケーション**やシンプルなスクリプトの作成を想定しています。クライアントのセットアップ、ログイン、Deposit・Withdrawal・トランザクション履歴の管理まですべてを網羅しています。

## 例: Node.js (TypeScript)

```bash
mkdir my-app
cd my-app
npm init -y
npm i --save-dev typescript tsx
npm i dotenv
```

### インストール

任意のパッケージマネージャーで SDK をインストールします：

```bash
# npm
npm i intmax2-server-sdk

# yarn
yarn add intmax2-server-sdk

# pnpm
pnpm i intmax2-server-sdk
```

### INTMAX Client の初期化

`INTMAXClient` は INTMAX SDK のコアコンポーネントで、INTMAX ネットワークとのシームレスな連携を提供します。このクラスにより、INTMAX ネットワークとのアプリケーション統合が簡素化され、`mainnet` と `testnet` の両環境で容易に連携できます。

```tsx
import { IntMaxNodeClient } from "intmax2-server-sdk";

const client = new IntMaxNodeClient({
  environment: "testnet",
  eth_private_key: process.env.ETH_PRIVATE_KEY,
  l1_rpc_url: process.env.L1_RPC_URL,
  urls: {
    balance_prover_url: "http://localhost:9001",
    use_private_zkp_server: false, // When using the balance prover locally on localhost, set `use_private_zkp_server` to false.
  }, // (Optional) URL of the balance prover service
});
```

- `environment`（String）：使用するネットワーク環境（例：`testnet` または `mainnet`）
- `eth_private_key`（String）：トランザクション署名に使用する Ethereum の秘密鍵（Private Key）
- `l1_rpc_url`（String）：Ethereum の RPC エンドポイント URL（例：Infura または Alchemy）

### ログイン / アカウントリカバリー

SDK の他の機能を使用するには、まずログインしてトークン残高を取得する必要があります。これにより、クライアントがユーザーの現在のアカウント状態と同期されます。

```tsx
await client.login();
const { balances } = await client.fetchTokenBalances();
```

### ログアウト / セッションリセット

ログイン状態をローカルでクリアし、サーバー上のセッションを無効化します。

```tsx
await client.logout();
```

### まとめ

以下のコードにより、アカウント情報と残高にアクセスできます。

```tsx
import { IntMaxNodeClient } from "intmax2-server-sdk";
import "dotenv/config";

const main = async () => {
  const client = new IntMaxNodeClient({
    environment: "testnet",
    eth_private_key: process.env.ETH_PRIVATE_KEY,
    l1_rpc_url: process.env.L1_RPC_URL,
  });
  await client.login();
  const { balances } = await client.fetchTokenBalances();
  console.log(`Balances of ${client.address}:`);
  console.log(balances);
  await client.logout();
};

main()
  .then(() => {
    process.exit(0);
  })
  .catch((error) => {
    console.error("Error:", error);
    process.exit(1);
  });
```

### 環境変数

使用する Ethereum の秘密鍵を `ETH_PRIVATE_KEY` に指定してください。

`L1_RPC_URL` には、テストネットを使用する場合、`Sepolia` ネットワークに接続可能なものを指定してください。

```bash
# .env
ETH_PRIVATE_KEY="0x..."
L1_RPC_URL="https://sepolia.gateway.tenderly.co"
```

環境変数を設定したら、以下のコマンドで実行できます。

```bash
npx tsx src/index.ts
```

### 次のステップ

**intmax2-server-sdk** では、Deposit・Withdrawal・Transfer やトランザクション履歴の取得機能も提供しています。詳細は SDK のドキュメントを参照してください。

[Client SDK リファレンスを見る](developers-hub/intmax-client-sdk/overview.md)

## Tips: ローカル Balance Prover の実行方法

ローカル Balance Prover のセットアップについては、Tips: [How to Run a Local Balance Prover](https://github.com/InternetMaximalism/intmax2-client-sdk/blob/main/README.md#tips-how-to-run-a-local-balance-prover) を参照してください。
