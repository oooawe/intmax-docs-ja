---
icon: rotate
description: Privacy Mining の Redeposit 手順
---

# Redeposit

Redeposit 機能について、以下の点に留意してください：

- 最初のマイニングラウンドが完了した後、自動的に次のラウンドに資金を Deposit できる機能です。
- Withdrawal のために新しいウォレットを繰り返し作成する手間を省くために用意されています。
- 資産の Withdrawal（システムが生成するアドレスへの送金）、`ITX` の Claim、そして Redeposit の 3 つのプロセスを一括で行います。
- システムが生成したアドレスにガス代を補充する必要があります。これにより、Redeposit のタイミングが来た際にシステムが資金を送出できます。
- ガス代の急激な変動により、2 回以上のガス補充が必要になる場合があります。
- Redeposit は最初の 2 つのプロセス（Withdrawal と Claim）が正常に完了した後に開始されます。その後、システムが生成したアドレスに送られた資金を Deposit し、次のマイニングラウンドに使用します。

## ステップバイステップガイド

### マイニングセッションの選択

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_10.png" alt="Redeposit" width="75%"></div>

上の画像のマイニングポートフォリオから、「Ready to Claim」ステータスのマイニングセッションをクリックします。このステータスは、マイニング期間が経過し、資産の Withdrawal や Redeposit、およびリワードの Claim が同時に行えることを意味します。

### リワード Claim の開始

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_20.png" alt="Redeposit" width="75%"></div>

上のページには Withdraw と Redeposit のオプションがあります。Redeposit をクリックして Redeposit 機能を実行します。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_30.png" alt="Redeposit" width="75%"></div>

Redeposit を進めるには、まず蓄積されたリワードを Claim する必要があります。「Proceed」をクリックして続行します。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_40.png" alt="Redeposit" width="75%"></div>

メッセージに署名します：

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_50.png" alt="Redeposit" width="75%"></div>

この段階では、システムが残高を確認し、資金が利用可能かどうかを検証しています。約 40 秒お待ちください。画面は自動的に遷移します。

**注意**：INTMAX はステートレスであるため、データはクライアント側に保存されており、システムはユーザーのアカウント状態を把握していません。そのため、この検証プロセスが必要です。

### ガス代の補充

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_60.png" alt="Redeposit" width="75%"></div>

資金が利用可能であることが確認されました。Redeposit トランザクションに必要なガス代を補充します。

**注意**：ガス代の急激な変動により、2 回以上のガス補充が必要になる場合があります。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_70.png" alt="Redeposit" width="75%"></div>

ガス補充のトランザクションを確認します：

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_80.png" alt="Redeposit" width="75%"></div>

トランザクションに署名します：

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_90.png" alt="Redeposit" width="75%"></div>

左側のインジケーターが緑色に変わるまで、このページでお待ちください。緑色に変わるまでは、ページを閉じたり離れたりしないでください。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_110.png" alt="Redeposit" width="75%"></div>

両方のインジケーターが緑色になったら、ページを離れるか、上のボタンをクリックしてアクティブなマイニングセッションに移動できます。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_120.png" alt="Redeposit" width="75%"></div>

マイニングページに戻ると、トランザクションのステータスが「Processing fee top up」に更新されています。クリックすると詳細を確認できます。

5 分以上待ってもステータスが更新されない場合は、ページを再読み込みするか、ウォレットの再接続を試してください。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_130.png" alt="Redeposit" width="75%"></div>

**注意**：この時点では、リワードの Claim と `ETH` の Withdrawal はまだ実行されていません。

### マイニングステータスの確認

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_140.png" alt="Redeposit" width="75%"></div>

ガス代の処理が完了すると、ステータスが「Ready to Claim」に戻ります。「Ready to Claim」ステータスのマイニングセッションをクリックします。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_150.png" alt="Redeposit" width="75%"></div>

再度 Redeposit をクリックして、Redeposit アクションを実行します。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_160.png" alt="Redeposit" width="75%"></div>

Redeposit を進めるには、まず蓄積されたリワードを Claim する必要があります。「Proceed」をクリックして続行します。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_170.png" alt="Redeposit" width="75%"></div>

メッセージに署名します：

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_180.png" alt="Redeposit" width="75%"></div>

この段階で、システムは再び残高を確認し、資金がまだ利用可能かどうかを検証します。約 40 秒お待ちください。画面は自動的に遷移します。

**注意**：INTMAX はステートレスであるため、データはクライアント側に保存されており、システムはユーザーのアカウント状態を把握していません。そのため、この検証プロセスが必要です。

### `ETH` の Withdrawal と `ITX` の Claim

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_190.png" alt="Redeposit" width="75%"></div>

このフォームでは、Withdrawal 先アドレスにシステムが自動生成したアドレスがあらかじめ入力されています。リワードの受取先として、接続中のウォレットアドレスが自動的に入力されています。別のアドレスを使用したい場合は、手動で変更してください。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_200.png" alt="Redeposit" width="75%"></div>

Redeposit 用の Withdrawal アドレスは、システムが Redeposit のために自動生成するため、編集できません。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_210.png" alt="Redeposit" width="75%"></div>

Withdrawal の詳細を再確認し、内容が正しいことを確認します。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_220.png" alt="Redeposit" width="75%"></div>

この画面で数分間お待ちください。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_230.png" alt="Redeposit" width="75%"></div>

トランザクションに署名します：

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_240.png" alt="Redeposit" width="75%"></div>

左側のインジケーターが緑色に変わると、Withdrawal が正常に開始されたことを意味します。ページを安全に閉じることができます。「Show detailed process」をクリックすると、プロセスのステップごとの内訳とステータスを確認できます。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_250.png" alt="Redeposit" width="75%"></div>

トランザクションの完了を待つ間、このページを閉じても問題ありません。

### 重要な注意事項

- トランザクション処理中は、約 2 分間 INTMAX の Web サイトを閉じたりページから離れたりしないでください。途中で Web ページを閉じると、トランザクションがキャンセルされ、Withdrawal プロセスを最初からやり直す必要があります。
- 画面左側に表示されるトランザクションインジケーターが緑色に変わると、Withdrawal が正常に開始されたことを意味します。インジケーターが緑色になって初めて、トランザクションに影響なく Web ページを閉じることができます。
- 資産の到着時間は資産の種類によって異なります。Withdrawal した `ETH` は通常、最大 11 時間以内に届きます。`ITX` トークンは、Withdrawal リクエストが正常に処理された翌日の UTC 00:00 に届く予定です。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_260.png" alt="Redeposit" width="75%"></div>

Withdrawal と Claim が開始されると、マイニングセッションのステータスが「Claim Processing」に更新されます。リワード（`ITX` トークン）は、Withdrawal リクエストが正常に処理された翌日の UTC 00:00 に届く予定です。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_270.png" alt="Redeposit" width="75%"></div>

### 再度 Deposit する

「Completed」と表示されたら、該当するマイニングセッションをクリックして、Redeposit の適格条件を満たしているか確認します。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_280.png" alt="Redeposit" width="75%"></div>

適格性が確認できたら、「Proceed Redeposit」ボタンをクリックして進みます。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_290.png" alt="Redeposit" width="75%"></div>

「Redeposit」ボタンをクリックしてアクションを確定し、トランザクションの処理を待ちます。

<div align="center" data-with-frame="true"><img src="assets/user-guides/redeposit_300.png" alt="Redeposit" width="75%"></div>

「Completed」と表示されたら、該当するマイニングセッションをクリックして Redeposit の適格条件を満たしているか確認します。適格性が確認できたら、「Proceed Redeposit」ボタンをクリックして進みます。

最後に「Redeposit」ボタンをもう一度クリックしてアクションを確定します。トランザクションが完全に処理されるまでお待ちください。

以上の手順に従うことで、リワードの Redeposit が正常に完了し、INTMAX Network でのマイニングを最適化できます。
