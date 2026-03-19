---
icon: wand-magic-sparkles
description: INTMAX Client SDK を活用したアイデア発想・計画・プレゼン準備用の AI プロンプト集
---

# Vibe Coding — 汎用プロンプト集

## はじめに

INTMAX Client SDK 向け VibeCoding へようこそ！

このガイドは、**AI** と **INTMAX Network** を組み合わせて、**アイデアを形にする** ための手引きです。アイデアの発想、検証、プロトタイプやアプリケーションの構築を素早く進められるよう設計されています。セットアップや計画に時間をかけるのではなく、**構築と反復に集中** できます。

このガイドに沿って進めることで、以下が可能になります：

- プロジェクトアイデアの発想と洗練
- 開発プロセスの計画
- 最小限の動作するプロトタイプの作成
- 明確なプレゼンテーションとデモの準備
- 改善のための体系的なフィードバックの取得

## フロー概要

このフローの後は、**VibeCoding for Coding Prompt** に進めます。

[Vibe Coding — 開発プロンプト集](./vibe-coding-for-coding-prompt.md)

こちらでは、**スターターテンプレート**（Vite + TypeScript、Next.js、Node.js 等）を素早くスキャフォールドし、セットアップではなく **実際の開発** に集中できます。

> **ガイダンス：**
>
> - 下記のスタータープロンプトを確認してください。
> - `intmax2-client-sdk` を使った最小限の動作する dApp テンプレートを生成してください。
> - すぐにコーディングを開始し、反復してください。

```
Documentation
|
v
Welcome Message
|
v
Common / Useful Prompts
|
├── Expansion Prompt (Idea Generation)
|         |
|         v
|    Idea Validation Prompt
|
├── Planning Prompt
|
├── Market & Use Case Prompt
|
├── Presentation Prep Prompt
|
v
Feedback Prompt
```

## 汎用プロンプト集

### **Expansion プロンプト（アイデア発想）**

```
You are an expert blockchain developer and product strategist.

1. Explain the key features and advantages of the INTMAX Network in clear, beginner-friendly language.

   * Use the official documentation (https://docs.network.intmax.io/, https://docs.network.intmax.io/developers-hub, https://intmax.io/) as reference sources.
   * Cover these aspects:

     * Stateless Layer-2 architecture (zk-Rollup + Plasma hybrid)
     * Ultra-low on-chain data usage (~5 bytes per transaction)
     * Scalability and near-instant finality
     * Built-in privacy (zero-knowledge proofs, privacy mining, anonymity guarantees)
     * Client-side storage and wallet interoperability (Client SDK, Wallet SDK)
     * Privacy mining incentive mechanism with ITX tokens
2. Suggest 5 types of applications or dApps that would be a good fit to build on INTMAX Network.

   * Prioritize apps that highlight its unique strengths like scalability, privacy, micro/bulk transfers, and cross-chain UX.
   * For each idea:

     * Describe what the app does
     * Explain why INTMAX is especially well-suited for it
     * Identify the type of users who would benefit the most

Make the response structured, practical, and inspiring so developers can quickly understand and pick an idea to build.
```

---

### **アイデア検証プロンプト**

```
Here is my project idea: [describe your idea briefly].

Please evaluate:

1. Technical feasibility.
2. Uniqueness compared to similar existing products.
3. Potential impact and user value.
4. Simple improvements to make the idea stronger.
```

---

### **計画プロンプト**

```
I want to plan the development process for my project.

Please create a step-by-step plan with milestones, including:

* Initial setup and environment configuration
* Core feature development
* Testing and debugging
* Final polishing and documentation
```

---

### **市場・ユースケースプロンプト**

```
I want to understand possible real-world applications of my project idea.
Describe 3 concrete use cases for: [your project idea].

For each use case:

* Who would benefit (target users)
* Why this problem matters
* How INTMAX + AI makes it better
```

---

### **プレゼン準備プロンプト**

```
I need to prepare a short presentation to introduce my project.

Please draft a presentation outline with this flow:

1. Problem (why this matters)
2. Solution (our AI + INTMAX approach)
3. Demo (what we will show)
4. Future potential (what comes next)

Keep it simple, clear, and focused on the core message.
```

---

### **フィードバックプロンプト**

```
Imagine you are a mentor or reviewer.
Here is my project: [describe idea].

Please give feedback from the perspective of:

* Innovation
* Practicality
* User experience
* Future potential
```

---

## 参考リンク

- **INTMAX 公式ドキュメント：** [https://docs.network.intmax.io/](https://docs.network.intmax.io/)
- **Developers Hub：** [https://docs.network.intmax.io/developers-hub](https://docs.network.intmax.io/developers-hub)
- **ウェブサイト：** [https://intmax.io/](https://intmax.io/)
