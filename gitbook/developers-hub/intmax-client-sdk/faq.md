---
icon: comments-question-check
description: INTMAX Client SDK のよくある質問と回答
---

# FAQ

INTMAX Client SDK に関するよくある質問とその回答をまとめたページです。アカウント管理、手数料、トランザクション、セキュリティ、プライバシーなど、開発者からの一般的な問い合わせに対する説明を提供します。

### Q. INTMAX ネットワークはスマートコントラクトをサポートしていますか？

いいえ、INTMAX ネットワークはスマートコントラクトを **サポートしていません**。代わりに、**Client SDK** を通じてネットワークとの連携を行います。SDK は、トランザクションの送信、資産管理、アプリケーション統合に必要なすべての機能を提供します。

### Q. INTMAX ネットワークにおける「ログイン」とは何ですか？ {#q-what-is-login-in-the-context-of-the-intmax-network}

**ログイン** は、指定された Ethereum アカウントから INTMAX アドレスを生成するプロセスです。INTMAX アドレスは、一般的な EVM チェーンとは異なる署名スキームを使用します。ブラウザ上で EVM 互換のウォレットアプリケーションを通じてアクセスします。

### Q. Ethereum アドレスと INTMAX アドレスは同じですか？

いいえ、異なるものです。詳細は [アカウントシステム](/gitbook/developers-hub/intmax-client-sdk/overview.md#account-system) を参照してください。

### Q. INTMAX ネットワークの手数料はどのように決まりますか？ {#q-how-are-transaction-fees-determined-on-the-intmax-network}

INTMAX ネットワークの手数料は以下の場合に発生します：

- INTMAX ネットワーク内での Transfer
- INTMAX から Ethereum への Withdrawal
- マイニングリワードの Claim

現在、SDK はマイニングを扱いません。そのため、ここで説明する手数料体系は、ネットワーク上の Transfer と Withdrawal にのみ適用されます。

- Transfer 手数料:
  - 初回トランザクション: **2,250 - 2,500 Gwei**
  - 2 回目以降のトランザクション: **1,800 - 2,000 Gwei**
- Withdrawal 手数料: **32,500 Gwei**

### Q. 複数のトランザクションをバッチ処理した場合、手数料はどうなりますか？ {#q-what-happens-to-transaction-fees-when-multiple-transactions-are-batched-together}

多数のトランザクション（例えば 63 件）を 1 つのブロックにバッチ処理した場合でも、個々のトランザクションの手数料体系は同じです。

### Q. `broadcastTransaction` と `withdraw` の戻り値はどう使いますか？ {#q-how-do-we-use-the-return-value-of-broadcasttransaction-and-withdraw}

`broadcastTransaction` と `withdraw` 関数は以下のようなレスポンスを返します：

```json
{
  "txTreeRoot": "0x52146f411e84ccba11e0887a0780a558f41042300a1515c7ff2cb7e1dd8b8c77",
  "transferDigests": [
    "0x0fddb7a7b18025c8a2242a66c8c73100f272ba0fc0064c65d725badcc5f9df66",
    "0xbccada67a9ad5eafae682fe000c955b6fd2bde90b16298dac87aa23bd021aa65"
  ]
}
```

この戻り値は `waitForTransactionConfirmation` 関数で使用します。この関数により、トランザクションが確認されるまで待機できます。
詳細は [API リファレンスの waitForTransactionConfirmation セクション](/gitbook/developers-hub/intmax-client-sdk/api-reference.md#waitfortransactionconfirmation) を参照してください。

```ts
const transferConfirmation = await client.waitForTransactionConfirmation({ txTreeRoot });
```

### Q. コラテラル手数料とは何ですか？ {#q-what-is-the-collateral-fee}

**コラテラル手数料（collateral fee）** は、ユーザーからのスパム攻撃を防ぐために Block Builder が導入した手数料です。

ユーザーが Transfer を途中でキャンセルした場合、コラテラル手数料として指定された金額が課金されます。Transfer がキャンセルされずに完了した場合、コラテラル手数料は使用されません。

コラテラル手数料は通常、通常の手数料の 2〜10 倍に設定されます。Transfer を開始するには、通常手数料またはコラテラル手数料のいずれか大きい方以上の残高が必要です。

### Q. Transfer とトランザクションの違いは何ですか？ {#q-what-is-the-difference-between-a-transfer-and-a-transaction}

INTMAX ネットワークでは、**Transfer** は 1 人の送信者から特定の 1 人の受取人へのトークン移動を指します。一方、**トランザクション** は同じ送信者から発信された複数の Transfer を 1 つのグループ操作にまとめたもので、複数の受取人が同時にトークンを受け取れます。

### Q. `claimWithdrawal` とは何ですか？ {#q-what-is-claimwithdrawal}

Withdrawal には **ネイティブトークン** と **非ネイティブトークン** の 2 種類があります。

- **ネイティブトークン** の場合、`withdraw` を呼び出すだけで十分です。トークンは指定されたアドレスに直接送信されます。
- **非ネイティブトークン** の場合、`withdraw` を呼び出した後、Withdrawal ステータスが `NeedToClaim` になります。この場合、プロセスを完了するために `claimWithdrawal` を明示的に呼び出す必要があります。

複数の保留中の Withdrawal をバッチ処理し、`claimWithdrawal` でまとめて Claim することもできます。

### Q. `tokenList` と `tokenBalances` とは何ですか？ {#q-what-are-tokenlist-and-tokenbalances}

- `tokenList` は、INTMAX ネットワーク上に存在するトークンの一覧です。

  INTMAX ネットワークに一度でも Deposit されたトークンはインデックス化され、一意の `tokenIndex` ID が割り当てられます。

- `tokenBalances` は、特定のアドレスが保有するすべてのトークンタイプと、各トークンの残高を表します。

### Q. INTMAX におけるプライバシーとは何を意味しますか？

INTMAX は強力なプライバシー保護を前提に設計されています。ウォレットオーナーのみが自身の資産残高とトランザクション履歴を閲覧できます。つまり、特定のアドレスの秘密鍵（Private Key）がなければ、ネットワーク参加者を含め、誰もこの情報にアクセスできません。

### Q. `broadcastTransaction` 関数の実行に時間がかかるのはなぜですか？

送金や Withdrawal などの操作では、ユーザーの現在の正確な残高に基づいて実行可能かどうかを検証する必要があります。最新ブロックの有効性が検証されるまで待つ必要があるため、`broadcastTransaction` 関数には一定の時間がかかります。
Withdrawal の場合、トランザクションのブロードキャスト後に、最新の状態を反映するために残高を再同期する必要があります。これは、Withdrawal プロセスを完了するためにノードへの追加リクエストが必要なためです。

server-sdk では、`sync` 関数を使って事前に残高同期を完了しておくことで、`broadcastTransaction` と `withdraw` 関数の実行時間を短縮できます。詳細は [Node.js の使用例](/gitbook/developers-hub/intmax-client-sdk/examples.md#notes-for-using-nodejs) を参照してください。

各関数のおおよその実行時間は以下の通りです。
ネットワークの混雑時にはさらに時間がかかる場合があります。

## メインネット

| 操作 | 時間（秒） |
| ----------------------------------------------- | -------- |
| broadcastTransaction（sync 前） | 164 |
| broadcastTransaction（sync 後） | 23 |
| waitForTransactionConfirmation（Transfer 後） | 50 |
| withdraw（sync 前） | 302 |
| withdraw（sync 後） | 187 |
| sync（Transfer 後） | 139 |

## テストネット

| 操作 | 時間（秒） |
| ----------------------------------------------- | -------- |
| broadcastTransaction（sync 前） | 256 |
| broadcastTransaction（sync 後） | 52 |
| waitForTransactionConfirmation（Transfer 後） | 50 |
| withdraw（sync 前） | 472 |
| withdraw（sync 後） | 257 |
| sync（Transfer 後） | 216 |
