---
icon: file-contract
description: INTMAX を構成するスマートコントラクト一覧と役割
---

# スマートコントラクト

このドキュメントでは、INTMAX プロトコルで使用されるスマートコントラクトについて説明します。

## Mainnet

2025 年 6 月 21 日更新

### Ethereum Mainnet

**Liquidity**

INTMAX Network への Deposit の受け入れと、INTMAX Network からのトークン Withdrawal を処理するコントラクトです。

[0xF65e73aAc9182e353600a916a6c7681F810f79C3](https://etherscan.io/address/0xF65e73aAc9182e353600a916a6c7681F810f79C3)

### Scroll Mainnet

**Rollup**

INTMAX Network で生成されたブロックを記録し、トランザクションをファイナライズするコントラクトです。

[0x1c88459D014e571c332BF9199aD2D35C93219A2e](https://scrollscan.com/address/0x1c88459D014e571c332BF9199aD2D35C93219A2e)

**Withdrawal**

INTMAX Network から Ethereum への Withdrawal プロセスを管理し、Withdrawal プルーフの検証とダイレクト Withdrawal の対象となるトークンインデックスの管理を行うコントラクトです。

[0x86B06D2604D9A6f9760E8f691F86d5B2a7C9c449](https://scrollscan.com/address/0x86B06D2604D9A6f9760E8f691F86d5B2a7C9c449)

**Claim**

Privacy Mining のリワードを Claim するためのコントラクトです。Withdrawal コントラクトと同じ手続きに従い、Ethereum 上で ITX トークンを分配します。

[0x22ac649b3229eC099C32D790e9e46FbA2CE6C9A5](https://scrollscan.com/address/0x22ac649b3229eC099C32D790e9e46FbA2CE6C9A5)

**Block Builder Registry**

アクティブな Block Builder を、関連する URL とともにハートビートシグナルを発信して登録するコントラクトです。Block Builder が自身の稼働状態を公に示すことができます。

[0x79dA6F756D26c50bA74bF9634bd3543645058b5B](https://scrollscan.com/address/0x79dA6F756D26c50bA74bF9634bd3543645058b5B)

## Testnet β

2025 年 5 月 24 日更新

### Ethereum Sepolia Network

**Liquidity**

INTMAX Network への Deposit の受け入れと、INTMAX Network からのトークン Withdrawal を処理するコントラクトです。

[0x81f3843aF1FBaB046B771f0d440C04EBB2b7513F](https://sepolia.etherscan.io/address/0x81f3843aF1FBaB046B771f0d440C04EBB2b7513F)

### Scroll Sepolia Network

**Rollup**

INTMAX Network で生成されたブロックを記録し、トランザクションをファイナライズするコントラクトです。

[0xcEC03800074d0ac0854bF1f34153cc4c8bAEeB1E](https://sepolia.scrollscan.com/address/0xcEC03800074d0ac0854bF1f34153cc4c8bAEeB1E)

**Withdrawal**

INTMAX Network から Ethereum への Withdrawal プロセスを管理し、Withdrawal プルーフの検証とダイレクト Withdrawal の対象となるトークンインデックスの管理を行うコントラクトです。

[0x914aBB5c7ea6352B618eb5FF61F42b96AD0325e7](https://sepolia.scrollscan.com/address/0x914aBB5c7ea6352B618eb5FF61F42b96AD0325e7)

**Claim**

Privacy Mining のリワードを Claim するためのコントラクトです。Withdrawal コントラクトと同じ手続きに従い、Ethereum 上で ITX トークンを分配します。

[0xceAa521Cb45d265831cBaF39E00875B550861A68](https://sepolia.scrollscan.com/address/0xceAa521Cb45d265831cBaF39E00875B550861A68)

**Block Builder Registry**

アクティブな Block Builder を、関連する URL とともにハートビートシグナルを発信して登録するコントラクトです。Block Builder が自身の稼働状態を公に示すことができます。

[0x93a41F47ed161AB2bc58801F07055f2f05dfc74E](https://sepolia.scrollscan.com/address/0x93a41F47ed161AB2bc58801F07055f2f05dfc74E)
