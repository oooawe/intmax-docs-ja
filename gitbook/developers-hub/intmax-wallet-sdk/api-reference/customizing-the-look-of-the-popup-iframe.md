---
icon: palette
description: dApp のブランドに合わせたポップアップ/iframe の外観カスタマイズ方法
---

# ポップアップ/iframe の外観カスタマイズ

INTMAX Wallet SDK を dApp に統合する際、トランザクションの署名や送信時にユーザーが操作するポップアップや iframe の外観をカスタマイズできます。名前、ロゴ、その他のメタデータを設定し、ウォレットインターフェースを自社ブランドに合わせることが可能です。以下に、SDK を使用したカスタマイズ方法を示します。

## 使用例

**ポップアップ/iframe の外観をカスタマイズする手順：**

1. **dApp メタデータの定義**：dApp のメタデータを設定します。ポップアップ/iframe に表示される名前、説明、アイコンを含めます。icons 配列にはカスタムロゴを指定できます。

   ```javascript
   // Define the default wallet URL for the INTMAX wallet or alternative wallet
   const DEFAULT_WALLET_URL = "https://wallet.intmax.io";

   // Define the path to your preferred icon, assuming the logo.png is in the public folder
   const DEFAULT_DAPP_ICON = `${window.location.origin}/logo.png`;

   const DAPP_METADATA = {
     name: "My Custom DApp", // Use your brand/project name here
     description: "This dApp allows users to interact with our custom smart contract.", // Use an appropriate description here
     icons: [DEFAULT_DAPP_ICON], // Use the defined icon path to your icon or logo here
   };
   ```

2. **SDK 初期化のカスタマイズ**：SDK インスタンスの作成時に、カスタムメタデータとウォレット URL を渡します。ウォレットウィンドウの表示モード（`iframe` または `popup`）も指定できます。

   ```javascript
   // Function to create an SDK instance, taking the wallet URL as an argument
   const createsSDK = (walletUrl: string) => {
     return intmaxDappClient({
       wallet: {
         url: walletUrl,
         name: "Custom Wallet", // Name of wallet
         window: { mode: "popup" }, // Choose between 'iframe' or 'popup'
       },
       metadata: DAPP_METADATA, // Pass the DApp metadata
       providers: { eip155: ethereumProvider() }, // Define the Ethereum provider
     });
   };

   const sdk = createsSDK(DEFAULT_WALLET_URL);
   ```

3. **dApp への統合**：カスタマイズした SDK インスタンスを通常通り dApp で使用します。ユーザーがウォレットを操作する際に表示されるポップアップや iframe に、カスタムブランディングが反映されます。

   ```javascript
   const Voting = () => {
     const [accounts, setAccounts] = useState<string[]>([]);

     const handleConnect = async () => {
       const intmaxWalletProvider = sdk.provider("eip155:137"); // 137 is Polygon mainnet network, change to network of choice
       await intmaxWalletProvider.request({ method: "eth_requestAccounts", params: [] });
       const accounts = (await intmaxWalletProvider.request({ method: "eth_accounts", params: [] })) as string[];
       setAccounts(accounts);
     };

     return (
       <div>
         <button onClick={handleConnect}>
           {accounts.length > 0 ? `Connected: ${accounts[0]}` : 'Connect Wallet'}
         </button>
       </div>
     );
   };
   ```

## 重要事項

- **背景色とテキスト色**：ポップアップ/iframe の背景色とテキスト色は、dApp に統合されたウォレットから継承されます。これにより、アプリケーション全体で一貫したビジュアル体験が確保されます。
- **カスタマイズオプション**：`DAPP_METADATA` オブジェクトで dApp の名前、ロゴ、説明を設定することで、ポップアップ/iframe をカスタマイズできます。`icons` 配列には、ポップアップ/iframe に表示されるロゴ画像のパスを指定します。
- `logo.png` がプロジェクトの `public` ディレクトリに配置されていることを確認してください。これにより、絶対パスで画像が正しく配信されます。
- `createsSDK` 関数は、カスタムウォレット URL または必要に応じて `DEFAULT_WALLET_URL` を渡して呼び出せます。
