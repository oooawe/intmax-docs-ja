---
icon: book-open
description: INTMAX Client SDK の全プロパティ・メソッド・パラメータ・戻り値の詳細リファレンス
---

# API リファレンス

INTMAX Client SDK の包括的な API リファレンスです。プロパティ、メソッド、パラメータ、戻り値、使用例の詳細な説明を含み、アカウント管理、トランザクション、Deposit、Withdrawal、手数料などを網羅しています。

### プロパティ

#### `isLoggedIn: boolean`

ウォレットユーザーの現在の認証状態を示すプロパティです。ユーザーが認証プロセスを正常に完了し、アクティブなセッションを持っている場合に true を返します。トランザクションやデータアクセスなど、機密性の高い操作を実行できるかどうかの判定に重要です。

#### `address: string`

接続されたウォレットの Ethereum アドレスと1対1で対応する、**INTMAX ネットワーク上のアドレス**を表します。

#### `tokenBalances: TokenBalance[] | undefine`

ユーザーの現在のトークン残高を表します。トークンアドレス、残高、シンボル、小数桁数、名前を含みます。undefined の場合、残高はまだ取得されていません。

## 関数

このセクションでは、`IntMaxClient` SDK が提供するすべての関数とメソッドを説明します。各関数には、目的、パラメータ、戻り値、使用例の詳細な説明が含まれています。これらの API により、INTMAX ネットワークに対するアカウント管理、トランザクション、Deposit・Withdrawal 操作を直接行えます。

特に指定がない限り、すべてのメソッドは非同期です。WebAssembly を介した安全な暗号演算により、最適なパフォーマンスを実現するよう設計されています。

### 初期化

`IntMaxClient` は INTMAX SDK のコアコンポーネントで、INTMAX ネットワークとのシームレスな連携を提供します。environment には **testnet** または **mainnet** を指定してください。

```tsx
import { IntMaxClient } from "intmax2-client-sdk";

const intMaxClient = IntMaxClient.init({ environment: "mainnet" });
```

ローカル Balance Prover のセットアップについては、Tips: [How to Run a Local Balance Prover](https://github.com/InternetMaximalism/intmax2-client-sdk/blob/main/README.md#tips-how-to-run-a-local-balance-prover) を参照してください。

### アカウント

アカウントモジュールは、ウォレット管理のための包括的な認証機能と暗号演算を提供します。

#### `login`

ウォレットの認証を開始し、安全なセッションを確立します。このメソッドは鍵導出、セッショントークン管理、初期データ同期を処理します。ウォレットの保護された機能への安全なアクセスに不可欠です。

[**Q.** INTMAX ネットワークにおける「ログイン」とは？](developers-hub/intmax-client-sdk/faq.md#q-what-is-login-in-the-context-of-the-intmax-network)

```tsx
const loginResponse: LoginResponse = await client.login();

// example
{
  address: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG",
  isLoggedIn: true,
  nonce: 0,
  encryptionKey: "/Nu5eFDLrYh0eRrU2izrptQE28aCE15XaVsjpM2JOmQ=",
  accessToken: "eyJkbGciOiJXUzUxNiIsInR3cCI6IkqXVCJ9.eyJhZGRyZXNzIjoiMHhmOWM3OGRhZTAxYWY3MjdlMmY2ZGI5MTU1Yjk0MmQ4YWI2MzFkZjRiIiwiZRhwIjoxNyQ5NjE4NjY2ODg5fQ.6Xa7fy0dtQBPDQR6mEJ1buH1fZo-GP6Fn8SgTDA8hGG1ZwkfMmpo-S36OUnxjPyIo76Ds4qz6ChFH40Ix40hfA"
}
```

**高度な機能：**

`nonce` と `encryptionKey` を使用して秘密鍵（Private Key）を保護し、以降の使用時に外部ウォレットの署名（Signature）なしでリカバリーを可能にできます。この機能の使用は任意です。

`encryptionKey` は AES-GCM 暗号化のキーとして使用できる 32 バイトの文字列で、Base64 エンコードされた文字列として出力されます。

`nonce` は暗号化キーの生成に関連する値です。同じ nonce は常に同じ encryptionKey を生成します。

`encryptionKey` の値は、外部ウォレットによって署名された固定メッセージと nonce 値から導出されます。encryptionKey が漏洩するリスクがある場合は、nonce を更新して新しい暗号化キーを生成できます。

#### `logout`

現在のウォレットセッションを安全に終了し、メモリから機密データをクリアします。このメソッドは認証トークン、キャッシュデータ、アクティブな接続の適切なクリーンアップを保証します。ウォレット操作の終了時にセキュリティを維持するために重要です。

```tsx
const res: void = await client.logout();
```

#### `getPrivateKey`

署名操作に必要な場合に、INTMAX の秘密鍵を安全に取得します。ユーザーが **INTMAX の秘密鍵をバックアップ**したい場合に使用できます。この関数を実行しなくても、トランザクションの署名やトランザクション履歴の復号は引き続き可能です。

```tsx
const privateKey: string = await client.getPrivateKey(); // will return hex string of private key
```

#### `signMessage`

INTMAX アカウントを使用してメッセージに署名します。この関数は、任意の文字列であるメッセージに対して署名を行います。

```tsx
const message = "Hello, World!";
const signature: SignMessageResponse = await client.signMessage(message);

// example
[
  "0x04f4185d46acee320b4628d252dc5d4802999b4499c9260ba3db9a7201a833dd",
  "0x273118b123ab37016456a538b5e15b881642b1a278a211bbd778f95d3f3c062e",
  "0x1af8a1df154fde0fa6c48d6c5e36c0edea202eb4da3d21062938f10a026b0a06",
  "0x2619565e2f4173043a4030c9f12d7dbada6b363425d1580e7a96c0632ad42585",
];
```

**注意**: 署名は決定論的に計算されます。つまり、同じアカウントで同じメッセージに署名すると、常に同じ署名が生成されます。

#### `verifySignature`

`signMessage` 関数で生成された署名を検証し、元のメッセージと INTMAX アカウントに一致することを確認します。

```tsx
const signature = [
  "0x04f4185d46acee320b4628d252dc5d4802999b4499c9260ba3db9a7201a833dd",
  "0x273118b123ab37016456a538b5e15b881642b1a278a211bbd778f95d3f3c062e",
  "0x1af8a1df154fde0fa6c48d6c5e36c0edea202eb4da3d21062938f10a026b0a06",
  "0x2619565e2f4173043a4030c9f12d7dbada6b363425d1580e7a96c0632ad42585",
];
const message = "Hello, World!";
const isVerified: boolean = await client.verifySignature(signature, message);
```

#### `sync`

```ts
await intMaxClient.sync();
```

`sync` 関数は、ユーザーの残高を最新の状態に更新します。INTMAX ネットワークでは、Transfer や Withdrawal を行う前にユーザーの残高を更新する必要があります。

ただし、**フロントエンド**では通常、この関数を手動で呼び出す必要はありません。`IntMaxClient` のインスタンスが作成されると、`sync` 関数はバックグラウンドで定期的に自動実行されます。

**重要：**

* ⚠️ **フロントエンド**では、`sync` 関数を通常の使用で手動呼び出ししないでください。
* ⚠️ 複数の `sync` 呼び出しは同時実行できません — 同時に呼び出された場合、いずれかが失敗します。
* ✅ ただし、異なる秘密鍵で 2 つの別々の IntMaxClient インスタンスを作成した場合、それぞれの sync 関数を同時に呼び出すことは安全です。

#### `updateL1RpcUrl`

Deposit トランザクション実行時に使用する Ethereum（Sepolia）ネットワークの RPC URL をカスタマイズできます。

```tsx
const newL1RpcUrl = "https://new-rpc-url.com";
intMaxClient.updateL1RpcUrl(newL1RpcUrl);
```

### トークン

この SDK は、ウォレットエコシステム内の暗号資産やデジタルアセットの情報を管理します。

[Q. `tokenList` と `tokenBalances` とは？](developers-hub/intmax-client-sdk/faq.md#q-what-are-tokenlist-and-tokenbalances)

#### `getTokensList`

トークンの一覧を取得する API です。`tokenIndex` パラメータは、Deposit・Withdrawal・Transfer などの操作でトークンを指定する際に特に重要です。これにより、INTMAX Client SDK 内でトークンを正確に識別できます。

```tsx
const tokens: Token[] = await client.getTokensList();

// example
[
  {
    contractAddress: "0x08210f9170f89ab7658f0b5e3ff39b0e03c594d4",
    decimals: 18,
    image: "https://..../ethereum.png",
    price: 3000,
    symbol: "ETH",
    tokenIndex: 0,
  },
];
```

#### `fetchTokenBalances`

現在ログイン中の INTMAX アカウントが保有するすべてのトークン残高を取得します。アプリケーション内でユーザーの資産保有状況を表示する際に便利です。

```tsx
const tokenBalances: TokenBalancesResponse = await client.fetchTokenBalances();

// example
{
  balances: [
    {
      amount: 10000n,
      token: {
        contractAddress: "0x0000000000000000000000000000000000000000",
        decimals: 18,
        image: "https://.../ethereum.png",
        price: 3000,
        symbol: "ETH",
        tokenIndex: 0,
      },
    },
  ];
}
```

### トランザクション

INTMAX ネットワーク内の Transfer メカニズムでトランザクションを実行することで、ネットワーク内での Transfer が行われます。ETH、ERC-20、ERC-721、ERC-1155 トークンに対応しています。

#### `fetchTransfers`

ユーザーが受信したトークンの履歴を取得します。金額、送信者（`from`）、受信者（`to`）、ステータス、タイムスタンプ、トークン情報などの詳細が含まれます。

[Q. Transfer とトランザクションの違いは？](developers-hub/intmax-client-sdk/faq.md#q-what-is-the-difference-between-a-transfer-and-a-transaction)

```tsx
const transferList: FetchTransactionsResponse = await fetchTransfers({})

// example
[
 {
    amount: "100000000000000000",
    from: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG", // INTMAX address
    status: 2,
    timestamp: 1735364080,
    to: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG", // INTMAX address
    tokenIndex: 0,
    transfers: [],
    txType: "Receive"
    digest: "0xcb2a31dd08339e4a95c3ab03c9111888d81baa65f21413679b943af0c8fcd9b3"
  },
  {
    amount: "100000000000000000",
    from: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG",
    status: 2,
    timestamp: 1735364080,
    to: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG",
    tokenType: 1,
    tokenIndex: 7,
    transfers: [],
    txType: "Receive"
    digest: "0xcb2a31dd08339e4a95c3ab03c9111888d81baa65f21413679b943af0c8fcd9b3"
  }
]
```

#### `fetchTransactions`

ユーザーが送信したトークンの履歴を取得します。各トランザクションには、異なる受信者への複数の Transfer と手数料の支払いが含まれる場合があります。

トランザクションと Transfer の違いについては、以下の FAQ を参照してください。

[Q. Transfer とトランザクションの違いは？](developers-hub/intmax-client-sdk/faq.md#q-what-is-the-difference-between-a-transfer-and-a-transaction)

```tsx
const transactionList: FetchTransactionsResponse = await fetchTransactions({})

// example
[
 {
    amount: "",
    from: "",
    status: 2,
    timestamp: 1735364080,
    to: "",
    tokenIndex: 0,
    transfers: [
      {
        recipient: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG", // INTMAX address
        salt: "0x98dd88e41a4f86e210860c414e0b78426c75b4243e85d3500a82c5006bce4749",
        amount: "100000000",
        tokenIndex: 4,
        to: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG", // same as recipient
        isWithdrawal: false
      },
      {
        recipient: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG",
        salt: "0x5b287885b6629963a2a9b7a741aa02d69cbc27af4d7e5b07fa1c5cc7e0e089f4",
        amount: "1",
        tokenIndex: 0,
        to: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG",
        isWithdrawal: false
      }],
    txType: "Send"
    digest: "0x193d1e4afca1732c611b175076c60c6602d60c8143641d903f394c2372b215b6"
  }
]
```

#### `broadcastTransaction`

`broadcastTransaction` 関数は、1つ以上のトランザクションをブロックチェーンネットワークにブロードキャストします。受信者アドレス、送金額、トークンタイプなどのトランザクションパラメータの配列を受け取ります。トランザクションのブロードキャスト後、トランザクションツリーのルートを検証し、確認を待ちます。レスポンスには、確認ステータス、ブロック番号などの関連情報を含むトランザクション結果が含まれます。

1回の呼び出しで最大 **63 トランザクション**まで指定できます。

- [Q. INTMAX ネットワークのトランザクション手数料はどのように決まりますか？](developers-hub/intmax-client-sdk/faq.md#q-how-are-transaction-fees-determined-on-the-intmax-network)
- [Q. 複数のトランザクションをバッチ処理した場合、手数料はどうなりますか？](developers-hub/intmax-client-sdk/faq.md#q-what-happens-to-transaction-fees-when-multiple-transactions-are-batched-together)
- [Q. `broadcastTransaction` の戻り値はどのように使いますか？](developers-hub/intmax-client-sdk/faq.md#q-how-do-we-use-the-return-value-of-broadcasttransaction-and-withdraw)

```tsx
const params: BroadcastTransactionRequest[] = [
  {
    amount: 0.000001, // 0.000001 ETH
    token: {
      tokenType: 0, // TokenType.NATIVE
      tokenIndex: 0,
      decimals: 18,
      contractAddress: "0x0000000000000000000000000000000000000000",
      price: 2417.08
    },
    address: "T6ubiG36LmNce6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcFtvXnBB3bqice6uzcJU3h5JR5FWa72jBBLUGmEPx5VXcB3prnCZ", // INTMAX Address
  }
];
const isWithdrawal = false;

const transferResult = await client.broadcastTransaction(params, isWithdrawal);

// example
{
  txTreeRoot: "0x52146f411e84ccba11e0887a0780a558f41042300a1515c7ff2cb7e1dd8b8c77",
  transferDigests: [
    "0x0fddb7a7b18025c8a2242a66c8c73100f272ba0fc0064c65d725badcc5f9df66",
    "0xbccada67a9ad5eafae682fe000c955b6fd2bde90b16298dac87aa23bd021aa65"
  ]
}
```

#### `waitForTransactionConfirmation` {#waitfortransactionconfirmation}

`waitForTransactionConfirmation` 関数は、Transfer の実行後にトランザクションが完全にファイナライズされたかどうかを検証するために使用します。INTMAX ネットワークでは、トランザクションは `broadcastTransaction` 関数を使用してノードに送信され、その後処理されます。

`broadcastTransaction` の成功レスポンスだけでは、オンチェーンでのファイナライズを保証しません。そのため、`waitForTransactionConfirmation` 関数は、トランザクションのステータスが `success` または `failed` になるまで追跡する信頼性の高い方法を提供します。

**重要：**

* ⚠️ Transfer トランザクションの実行後に `waitForTransactionConfirmation` を呼び出すことが重要です。

**注意**: この関数は `withdraw` 関数の `txTreeRoot` でも使用できます。ただし、`withdraw` 関数の実行が完了した時点でトランザクションは既にオンチェーンに反映されているため、この関数でさらに待機する必要はありません。

```ts
const txTreeRoot = "0x52146f411e84ccba11e0887a0780a558f41042300a1515c7ff2cb7e1dd8b8c77";
const transferConfirmation = await client.waitForTransactionConfirmation({ txTreeRoot });
```

### Deposit

Ethereum メインネットから INTMAX ネットワークへの Deposit は、Ethereum メインネットの Liquidity Contract 上でトランザクションを実行することで行われます。ETH、ERC-20、ERC-721、ERC-1155 トークンに対応しています。

#### `fetchDeposits`

Deposit トランザクションのページネーション付きリストを、金額、送信者、受信者、ステータス、タイムスタンプ、トークン情報などの関連する詳細とともに取得します。このメソッドは、Deposit の追跡と管理のための包括的なデータを提供し、ユーザーがトランザクションのステータスと履歴を効率的に確認できるようにします。

```tsx
const depositList: Transaction[] = await client.fetchDeposits({})[
  // example
  {
    amount: "100000000000000000",
    from: "",
    status: 2,
    timestamp: 1735364080,
    to: "",
    tokenType: 3,
    tokenIndex: 0,
    transfers: [],
    txType: "Deposit",
    digest: "0x47f90ce9ee420a145e237397d106a35c6d3cf3c96c4f8eb2fa7caed26a6b2c17",
    tokenAddress: "0x1e4da6fb14da45f025622e501e28ade8dc5e4ec8",
  }
];
```

#### `deposit`

必要なトランザクションパラメータを使用して、ユーザーのアカウントへの Deposit トランザクションを処理します。このメソッドは、バリデーション、署名、ネットワークへのブロードキャストを含む完全な Deposit フローを処理し、**Predicate Contract を介したオンチェーン AML（Anti-Money Laundering）チェック**を含みます。

ここで、`estimateDepositGas` はトランザクションに十分なガスがあるかを事前に検証するために必要です。

`deposit` 関数は txHash と status の両方を返します。`status: 1` は処理中、`status: 2` は完了を表します。

```tsx
const params: PrepareDepositTransactionRequest = {
  amount: 0.000001, // 0.000001 ETH
  token: {
    tokenType: 0,
    tokenIndex: 0,
    decimals: 18,
    contractAddress: "0x0000000000000000000000000000000000000000",
  },
  address: "T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG", // INTMAX Address
  skipConfirmation: false,
}

// Check gas estimation to verify if the transaction can be executed
const gas: bigint = await client.estimateDepositGas({
  ...params,
  isGasEstimation: true,
});

// example
114231256530n

const deposit: PrepareDepositTransactionResponse = await client.deposit(params);

// example
{
  "status": 2,
  "txHash": "0xd03b99a0de83803bede24834715a36181008a73a76b627391042083c70af9c52" // Ethereum address
}
```

### Withdrawal

INTMAX ネットワークから Ethereum メインネットへの Withdrawal は、メインネットの Liquidity Contract 上でトランザクションを実行することで行われます。ETH、ERC-20、ERC-721、ERC-1155 トークンに対応しています。

#### `fetchWithdrawal`

Withdrawal トランザクションを、現在のステータスに基づいてカテゴリ別のリストに整理します。Failed、NeedClaim、Relayed、Requested、Success の各状態が含まれます。この構造化された整理により、Withdrawal の効率的なフィルタリングと管理が可能になり、ユーザーはトランザクションの進行状況を追跡し、各 Withdrawal 状態を個別に処理できます。各ステータスカテゴリには、詳細なトランザクション情報を含む ContractWithdrawal オブジェクトの配列が格納されます。

```tsx
const withdrawals = await client.fetchWithdrawals();

// example
{
 withdrawals: {
   failed: [],
   need_claim: [
     {
       recipient: "0xe888365f8b2ffe41Cc797F6Cb2dc25F191Aa9643", // Ethereum address
       nullifier: "0x1c66c5286f4281fd778a9f91ab3a7278b095aab77525c8383a4662ecd4569fc3",
       amount: "10",
       tokenIndex: 6
     }
   ],
   relayed: [],
   requested: [],
   success: [
     {
       recipient: "0xe888365f8b2ffe41Cc797F6Cb2dc25F191Aa9643", // Ethereum address
       nullifier: "0x802634b2292d5e3a3740f83e9d1817c78a3e23b866fcf08df1f747fb30610c41",
       amount: "52500000000000",
       tokenIndex: 0
     },
     {
       recipient: "0xC7f36AA3C3294025C1b6F57EBB93A0c81c86eEB3",
       nullifier: "0x8f72ab3f00af84b0860921a32d09e958847ebe53a16f33716ddb991d9efdf7ac",
       amount: "5250000000000",
       tokenIndex: 0
     }
   ]
 },
 pagination: undefined
}
```

#### `withdraw`

`withdraw` 関数は、ユーザーのウォレットからの Withdrawal リクエストを処理する非同期メソッドです。この関数は、Withdrawal が安全かつ正確に処理されるよう、包括的なバリデーションとセキュリティチェックを実行します。残高の検証、トランザクション手数料の計算、トランザクションの署名を含む、Withdrawal フロー全体を実行します。

`broadcastTransaction` 関数と同様に、`withdraw` 関数の実行後に `waitForTransactionConfirmation` 関数を使用してトランザクションのファイナライズを待つことができます。

```tsx

const params: WithdrawRequest = {
  amount: 0.00000001
  token: {
    tokenType: 0, // 0: NATIVE, 1: ERC20, 2: ERC721, 3: ERC1155
    tokenIndex: 0,
    decimals: 18,
    contractAddress: "0x0000000000000000000000000000000000000000",
  },
  address: "0xf9c78dAE01Af727E2F6Db9155B942D8ab631df4B", // ethereum address
}

const withdrawResult = await client.withdraw(params);

// example
{
  txTreeRoot: "0x4cd9d917b0cfc6b9a7073d44ed52256d734ff2f2011c0cf855967af12944dd4b",
  transferDigests: [
    "0x9f367ef1067d3b1abcb46de4afa1a99de622ab141dc64d07789a51e9a65df8dc",
    "0x70c9bae268e4da2e496058bd9c768bd83e618a9e10d34ef80901bb16389b5059",
    "0x124b88a1e566efd86078b2f9c305aacd83e56a686ae7731ee5b743a77421e580",
    "0x060f8bdd95502e610713529ec450da7ead1a8d85ebe5d3e81065ad22e3ab9672"
  ]
}
```

#### `claimWithdrawal`

1つ以上の Withdrawal トランザクションの Claim プロセスを開始し、トランザクションハッシュとステータスを含むレスポンスを返します。この関数の実行完了後、ブロックチェーン上での成功を確認するために Claim 操作の進行状況を追跡できます。

[Q. `claimWithdrawal` とは？](developers-hub/intmax-client-sdk/faq.md#q-what-is-claimwithdrawal)

```tsx
// The withdrawal will be processed in approximately three hours
const claim = await client.claimWithdrawal(withdrawals.need_claim);

// example
{
  status: 2,
  txHash: "0x305cb34f6c870e708b09e63cec9c02470bf5156b6e641c59237c84f46c1145b4" // Ethereum tx hash
}
```

**注意**: Claim 可能なトークンがない場合、`No withdrawals to claim` エラーが発生します。

## 技術用語

- **Nullifier** — 同一の Deposit / Withdrawal が二重使用されるのを防ぐための一意な識別子
- **Salt** — 暗号化やハッシュ処理時に追加されるランダム値。同一の入力から異なる出力を生成するために使用され、受信者の Deposit アドレスを秘匿する
- **Token index** — INTMAX ネットワーク内でトークンを一意に識別する数値 ID

## バッチサイズの制限 {#batch-size-limits}

INTMAX Client SDK は、最適なパフォーマンスの確保とリソース枯渇の防止のため、特定の操作にバッチサイズの制限を設けています：

### トランザクションのブロードキャスト

- **`broadcastTransaction` 1回あたりの最大トランザクション数**: 63
- この制限は、1回のトランザクションブロードキャストで複数の Transfer をバッチ処理する場合に適用されます

### データ取得操作

- **内部 API リクエストあたりの最大アイテム数**: 64
- この制限は以下の関数が使用する内部ページネーションに適用されます：
  - `fetchDeposits()`
  - `fetchTransfers()`
  - `fetchTransactions()`
- SDK がページネーションを自動処理するため、高レベル API の使用時にこの制限を管理する必要はありません
- 64 を超える値を指定すると、`Limit exceeds max batch size` または `Batch size exceeds maximum limit of 64` エラーが発生します

## 手数料

手数料の詳細については以下を参照してください。

- [Q. INTMAX ネットワークのトランザクション手数料はどのように決まりますか？](developers-hub/intmax-client-sdk/faq.md#q-how-are-transaction-fees-determined-on-the-intmax-network)
- [Q. コラテラル手数料（collateral fee）とは？](developers-hub/intmax-client-sdk/faq.md#q-what-is-the-collateral-fee)

#### `getTransferFee`

getTransferFee 関数は、INTMAX ネットワーク内でのトークン Transfer に必要なトランザクション手数料を見積もります。

```tsx
const trasnferFee: FeeResponse = await client.getTransferFee();

// example
{
  beneficiary: 'T7iFM2BEtd3JxkaUNfZge83CtAqhdgcjGgJpiityyrYMW739SyyDYF5bnR8fkSG3G9YT4VZtur3hKhvuDm5ZLneYLy8j7gG',
  fee: {
    amount: "2000000000000",
    token_index: 0,
  },
  collateral_fee: {
    amount: "20000000000000",
    token_index: 0,
  }
}
```

#### `getWithdrawalFee`

INTMAX ネットワーク内でのトークン Withdrawal に必要なトランザクション手数料を見積もります。

```tsx
const withdrawalFee: FeeResponse = await client.getWithdrawalFee();

// example
{
  beneficiary: 'T7v5ya7osVxjhu35X8hRMFD2wByq8tF2Pjkag2K2NfNMX8ZdUfiAD5pR2M4bQf8XgTAbAYzyLX6AMGm3Vj78oYNLH5PhGMm',
  fee: {
    amount: "32500000000000",
    token_index: 0,
  },
  collateral_fee: undefined
}
```
