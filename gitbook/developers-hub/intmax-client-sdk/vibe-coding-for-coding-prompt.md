---
icon: wand-magic-sparkles
description: INTMAX Client SDK のスターターテンプレートと開発用 AI プロンプト集
---

# Vibe Coding — 開発プロンプト集

## AI ツールで生産性を加速する

AI ペアプログラマーを使えば、ハッカソン中の開発速度が **飛躍的に向上** します。

以下のツールを **強く推奨** します：

- **Cursor**
- **Cline**
- **GitHub Copilot (VS Code)**
- **Gemini CLI**
- **Claude Code**
- **Replit**
- **v0**

最高性能のモデル：

- **GPT-5**
- **Gemini 2.5 Pro**
- **Claude Sonnet 4**

*ヒント：以下の Copilot スタータープロンプトをアシスタントに貼り付けると、素早くスキャフォールドできます。*

## スタータープロンプト

**タイトル：** `intmax2-client-sdk` を使った Vite + TypeScript スターター
（ウォレット接続と INTMAX2 残高表示。テストネット/メインネット切り替え対応）

**コンテキスト：**
VibeCoding コンテスト向けの最小限のウェブ dApp スターターを構築します。**`intmax2-client-sdk`** を使って以下を実装：

1. ウォレットの接続
2. INTMAX2 残高の取得と表示

**技術スタック（必須）：**

- Vite + React + TypeScript
- SDK: `intmax2-client-sdk`
- クリーンなモジュラー `/src` 構造
- `.env` で環境変数管理（シークレットのハードコード禁止）
- 最小限の UI（ヘッドレスまたは軽量 CSS 可）

**環境切り替え：**

- `VITE_INTMAX_ENV = 'testnet' | 'mainnet'`（デフォルト：**testnet**）
- 起動時にアクティブな環境に必要な変数を検証 → 不足時はエラー

---

### MVP 要件

1. **ウォレット接続**
   - `window.ethereum` を検出
   - ウォレット接続 → 省略されたアドレスを表示
2. **残高取得**
   - 「Refresh Balance」ボタン
   - INTMAX2 残高を表示
3. **UX の基本**
   - ローディング状態
   - インライン/トーストのエラーメッセージ
4. **型安全性**
   - `any` 型の使用禁止
5. **README**
   - **テストネット**（デフォルト）での実行手順
   - メインネットへの切り替え方法を明確に説明
   - MetaMask が必要である旨を明記

---

### プロジェクト構成

```
vite.config.ts
src/
  lib/
    env.ts         # Reads VITE_INTMAX_ENV & validates config
    sdk.ts         # Initializes SDK; exposes connectWallet/getBalance
  components/
    WalletConnect.tsx
    BalancePanel.tsx
  App.tsx
.env.local.example  # with testnet defaults + mainnet block
README.md
```

---

### `.env.local.example` の例

```
# Select environment: testnet (default) or mainnet
VITE_INTMAX_ENV=testnet
```

---

### README に含めるべき内容

**前提条件：** Node.js（LTS）、MetaMask（または EIP-1193 準拠ウォレット）

**テストネットで実行（デフォルト）：**

```bash
cp .env.local.example .env.local
npm i
npm run dev
```

**メインネットに切り替え：**

```bash
# edit .env.local
VITE_INTMAX_ENV=mainnet
# fill mainnet values
npm run dev
```

## 使い方（スキャフォールド）

1. **MetaMask がインストールされた** ブラウザでアプリを開く（ウォレットが検出されない場合は、下記のトラブルシューティングを参照）
2. **Connect Wallet** をクリック → MetaMask でリクエストを承認
3. **Refresh Balance** をクリック → 現在の INTMAX2 残高が表示される

**検証：** ウォレットアドレスと残高が表示されれば、セットアップは完了です。

## 次のステップ

スキャフォールドが動作したら、以下の拡張を検討してください：

- **基本的な拡張** → INTMAX Network へのトークン Deposit を行う「Deposit」ボタンの実装
- **UI のアップグレード** → 過去の残高やトランザクションの一覧を表示
- **AI 連携** → AI API（OpenAI、Claude、Gemini）と接続し、支払いを自動トリガー
- **創造性** → スキャフォールドをハッカソンのアイデア（P2P 決済、ゲーム、エージェント等）に拡張

このスキャフォールドはあくまで **出発点** です。

ハッカソンで **独自のプロダクトを構築して披露する** ことが目標です！

## トラブルシューティング（クイックガイド）

| 問題（表示内容） | 原因 | 解決方法 |
| ------------------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------- |
| ❌ **No wallet found** — 「Connect Wallet」ボタンが反応しない | ブラウザにプロバイダーが注入されていない | ✅ MetaMask（または EIP-1193 準拠ウォレット）をインストールしてページを更新 |
| ❌ **Missing env vars** — アプリが起動時にクラッシュする | `.env.local` が未設定または不完全 | ✅ `.env.local.example` を `.env.local` にコピーして必要な値を入力 |
| ❌ **Wrong chain** — ウォレットに「unsupported network」と表示される | ウォレットが別の L1 に接続されている | ✅ ウォレットのネットワークを必要な L1 チェーン（テストネット/メインネット）に切り替え |
| ❌ **RPC unreachable** — 残高が「0/unavailable」と表示される | RPC エンドポイントがダウンまたはブロックされている | ✅ 数秒後にリトライするか、DevTools コンソールでエラーを確認 |

## 汎用プロンプト集

### **デバッグプロンプト**

```
Here is my current code for the INTMAX agent.
It should send a payment, but fails.

1. Review for bugs or missing steps
2. Improve error handling & readability
3. Make it demo-ready
```

### **最適化プロンプト**

```
I have a working INTMAX agent prototype.

1. Reduce unnecessary calls
2. Improve UX flow
3. Suggest enhancements to impress judges
```

### **連携プロンプト（AI または外部 API の追加）**

```
I want to integrate an AI API (OpenAI, Claude, Gemini).

1. Show how to connect external AI in TypeScript
2. Example: AI makes a decision → triggers INTMAX payment
3. Keep demo-ready and simple
```
