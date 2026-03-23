---
icon: code
description: INTMAX Client SDK のブラウザ・Node.js 環境別コード例
---

# 使用例

INTMAX Client SDK をブラウザおよび Node.js 環境で使用するための実践的なコード例です。アカウントの初期化、認証、残高取得、トークン管理、トランザクション実行、Deposit、Withdrawal、ガス代の見積もり、メッセージの署名・検証を扱います。

### ブラウザでの使用

以下のモジュールについて JavaScript サポートを提供予定です。各モジュールの使用例を示します。

```bash
npm i intmax2-client-sdk
```

#### INTMAX Client の初期化

`IntMaxClient` は INTMAX SDK のコアコンポーネントで、INTMAX ネットワークとのシームレスな連携を提供します。このクラスにより、INTMAX ネットワークとのアプリケーション統合が簡素化され、`mainnet` と `testnet` の両環境で容易に連携できます。

```tsx
import { IntMaxClient } from "intmax-client-sdk";

const client = await IntMaxClient.init({ environment: "mainnet" });
```

ローカルに Balance Prover インスタンスをセットアップする場合は、Tips: [How to Run a Local Balance Prover](https://github.com/InternetMaximalism/intmax2-client-sdk/blob/main/README.md#tips-how-to-run-a-local-balance-prover) を参照してください。

#### INTMAX Network へのログイン

INTMAX へのログイン例です。SDK の各機能を使用する前に、一度ログインする必要があります。

2 つのメッセージに署名します。ポップアップウィンドウに自動的に表示されます：

1. `ETH` ウォレットアドレスを確認するメッセージに署名

2. チャレンジ文字列を含むメッセージに署名

```tsx
await client.login();
```

#### 残高の取得

生成された INTMAX アカウントの残高を取得する例です。

```tsx
const { balances } = await client.fetchTokenBalances();
```

### Node.js での使用

```bash
npm i intmax2-server-sdk
```

#### INTMAX Client の初期化

```tsx
import { IntMaxNodeClient } from "intmax2-server-sdk";

const client = new IntMaxNodeClient({
  environment: "mainnet",
  eth_private_key: "0x...", // Replace with your Ethereum private key
  l1_rpc_url: "https://sepolia.gateway.tenderly.co",
});
```

#### INTMAX Network へのログインと残高取得

INTMAX へのログインと残高取得の例です。SDK の各機能を使用する前に、一度残高を取得する必要があります。

```tsx
await client.login();
const { balances } = await client.fetchTokenBalances();
```

### 共通の使用例

#### INTMAX アカウントのアドレスと秘密鍵の取得

生成された INTMAX アカウントのアドレスと秘密鍵（Private Key）を取得する例です。

```tsx
const address = client.address; // Address of the INTMAX account
const privateKey = client.getPrivateKey(); // Private key of the INTMAX account
```

#### メッセージの署名と検証

メッセージに署名し、その署名（Signature）を検証する方法を示します。

```tsx
const message = "Hello, World!";
const signature = await client.signMessage(message);
const isVerified = await client.verifySignature(signature, message);
if (!isVerified) {
  throw new Error("Verification failed");
}

console.log("Verification succeeded");
```

#### 利用可能なトークンの一覧と特定トークン情報の取得

ネットワークでサポートされているトークンの一覧を取得する方法です。

```tsx
const tokens = await client.getTokensList();
console.log("Available tokens:", tokens);

const nativeToken = tokens.find(
  (t) => t.contractAddress.toLowerCase() === "0x0000000000000000000000000000000000000000",
);
```

#### トランザクション履歴の取得

Deposit、Transfer、送信済みトランザクションを並列で取得し、最新のエントリを表示します。

```tsx
const [deposits, transfers, sentTxs] = await Promise.all([
  client.fetchDeposits(),
  client.fetchTransfers(),
  client.fetchTransactions(),
]);

console.log("Deposits:", deposits);
console.log("Received Transfers:", transfers);
console.log("Sent Transfers:", sentTxs);
```

#### ガス代の見積もりと Deposit

`ETH` の Deposit に必要なガス代を見積もり、Deposit を実行します。

```tsx
const token = {
  tokenType: TokenType.NATIVE,
  tokenIndex: 0,
  decimals: 18,
  contractAddress: "0x0000000000000000000000000000000000000000",
};

const depositParams = {
  amount: 0.000001, // 0.000001 ETH
  token,
  address:
    "T6ubiG36LmNce6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcFtvXnBB3bqice6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcB3prnCZ", // recipient INTMAX address
};

// Dry-run gas estimation
const gas = await client.estimateDepositGas({
  ...depositParams,
  isGasEstimation: true,
});
console.log("Estimated gas:", gas);

// Execute the deposit
const depositResult = await client.deposit(depositParams);
console.log("Deposit result:", depositResult);
console.log("Transaction Hash:", depositResult.txHash);
```

取得した txHash は [SepoliaScan](https://sepolia.etherscan.io/) で検索できます。

#### Transfer 手数料の確認とトランザクションのブロードキャスト

現在の Transfer 手数料（トークンインデックス / 金額）を取得する例です。その後、0.000001 `ETH` を別の INTMAX アドレスに送信します。

```tsx
const token = {
  tokenType: TokenType.NATIVE,
  tokenIndex: 0,
  decimals: 18,
  contractAddress: "0x0000000000000000000000000000000000000000",
};

const transferFee = await client.getTransferFee();
console.log("Fee Token Index:", transferFee?.fee?.token_index);
console.log("Fee Amount:", transferFee?.fee?.amount);

const transfers = [
  {
    amount: 0.000001,
    token,
    address:
      "T6ubiG36LmNce6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcFtvXnBB3bqice6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcB3prnCZ", // recipient INTMAX address
  },
];

const transferResult = await client.broadcastTransaction(transfers);
console.log("Transfer result:", transferResult);
const transferConfirmation = await client.waitForTransactionConfirmation(transferResult);
console.log("Transfer confirmation result:", transferConfirmation);
```

#### Withdrawal 手数料の取得と Withdrawal の実行

Withdrawal 手数料と Transfer 手数料の両方を取得してから Withdrawal を実行する方法を示します。

`ETH` の Withdrawal を実行し、プルーフの準備が完了したら Claim します。

```tsx
const token = {
  tokenType: TokenType.NATIVE,
  tokenIndex: 0,
  decimals: 18,
  contractAddress: "0x0000000000000000000000000000000000000000",
};

// Withdrawal fee
const withdrawalFee = await client.getWithdrawalFee(token);
console.log("Withdrawal Fee Token:", withdrawalFee?.fee?.token_index);
console.log("Withdrawal Fee Amount:", withdrawalFee?.fee?.amount);

// Transfer fee (must also be paid)
const transferFee = await client.getTransferFee();
console.log("Transfer Fee Token:", transferFee?.fee?.token_index);
console.log("Transfer Fee Amount:", transferFee?.fee?.amount);

const withdrawalResult = await client.withdraw({
  address: "0xf9c78dAE01Af727E2F6Db9155B942D8ab631df4B", // Ethereum L1 address
  token,
  amount: 0.000001,
});
console.log("Withdrawal result:", withdrawalResult);

// Check pending withdrawals
const { withdrawals } = await client.fetchWithdrawals();
console.log("Withdrawals:", withdrawals);

// Claim once ready
const claim = await client.claimWithdrawal(withdrawals.need_claim);
console.log("Claim result:", claim);
```

### Node.js 使用時の注意事項 {#notes-for-using-nodejs}

server-sdk を使用する場合、`broadcastTransaction` と `withdraw` 関数の呼び出し前後に `sync` 関数を実行することを推奨します。

* **`sync` を呼び出さない場合:**
  次の Transfer や Withdrawal の前に残高は自動的に更新されますが、この自動更新には追加の時間がかかる場合があります。

* **事前に `sync` を呼び出す場合:**
  残高がすでに更新されているため、Transfer や Withdrawal をより速く開始できます。

* **Transfer の後:**
  `sync` を実行すると、完了したトランザクションが残高に反映され、次の Transfer がスムーズになります。

**重要:**

* ⚠️ 最良のエクスペリエンスのために、Transfer や Withdrawal の **前後** で必ず `sync` を実行してください。

#### Transfer の例

```ts
await client.sync();
const transferResult = await client.broadcastTransaction(params);
const transferConfirmation = await client.waitForTransactionConfirmation(transferResult);
await client.sync();
```

#### Withdrawal の例

```ts
await client.sync();
const withdrawResult = await client.withdraw(params);
await client.sync();
```

**注意**: **フロントエンド** では、通常の使用時に `sync` 関数を手動で呼び出す必要はありません。
