
■ハンズオン: 仮想マシンのリージョン間のレプリケーション

**東日本**のVMを**米国東部**にレプリケーションする。

**※注意: Azure Passサブスクリプションでは、西日本にVMを作成できないため、西日本をレプリケーション元/先とするハンズオンは実施できない。**

Windows仮想マシンを作成:

- リージョン: **東日本**
- 名前: win1
- リソースグループ: win1_group
- Windows Server 2019 Datacenter - Gen1
- ([smalldisk] Windows Server 2022 Datacenter - Gen2)
- Standard_D2s_v3 - 2 vcpu, 8 GiB メモリ
- 起動後、「エージェントの状態」が「Not Ready」から「Ready」になるまで2分ほど待つ。

レプリケーションの設定:

- 仮想マシン＞操作＞ディザスタリカバリー
  - 可用性ゾーン間のディザスタリカバリーを行いますか？: いいえ
  - ターゲットリージョン: **米国東部**
  - 次へ
  - （作成されるリソースを確認）
  - 「レプリケーションを確認して開始する」
  - 「レプリケーションの開始」
- （画面下部「ターゲットリソースを作成しています」と表示される。画面右上には通知が何度か表示される。この状態で2分ほど待つ）
- （画面が切り替わる）
- 「通知」（画面右上のベル）をクリックして状況を確認
  - 「1 個の VM のレプリケーションを有効にします」「操作が進行中です」と表示される
  - **10分ほど待つ**
  - 「1 個の VM のレプリケーションを有効にします」「操作を正常に完了しました。」と表示される

作成されたリソース:

- リソースグループ「Site-recovery-valut-RG」 - **米国東部**
  - ストレージアカウント - **東日本**
  - Automationアカウント - **米国東部**
  - Recovery Servicesコンテナー - **米国東部**
- リソースグループ「windows1_group_asr」（「（元のVMのリソースグループ名）-asr」となる） - **米国東部**
  - 仮想ネットワーク - **米国東部**
  - ディスク - **米国東部**

※Automationアカウントは、VMにインストールされる「Site Recovery モビリティ サービス拡張機能」の自動更新に使用される。
https://docs.microsoft.com/ja-jp/azure/site-recovery/azure-to-azure-autoupdate

以上でレプリケーションの設定は完了。以下の手順でレプリケーションの状況を確認できる。「プロテクト」になるまでの待ち時間が長いため、ここで長い休憩を取るとよい。

- Recovery Servicesコンテナー＞保護されたアイテム＞レプリケートされたアイテム＞windows1
  - 状態: 「保護を有効化中」や「0% 同期済み」などが表示される。「プロテクト」になるまで待つ（**1時間ほどかかる場合がある**）
  - 「正常に実行された最後のテスト フェールオーバー(Last successful Test Failover)」: 「正常に実行されませんでした(Never performed successfully)」と表示される。※テストフェールオーバーを行っていないのでこの表示で問題ない。

「最新の回復ポイント」をクリックすると、利用可能な回復ポイント（復旧ポイント）を確認することができる。

■ハンズオン: テストフェールオーバー

レプリケーションの状態が「プロテクト」となったら、テストフェールオーバーを実施できる。

テスト用の仮想ネットワークの作成:

- 仮想ネットワーク＞作成
  - リソースグループ: win1_group-asr
  - 名前: win1_group-vnet-asr-test
  - 地域(リージョン): **米国東部**

テストフェールオーバーの実行:

- Recovery Servicesコンテナー＞保護されたアイテム＞レプリケートされたアイテム＞windows1
- 「テスト フェールオーバー」
- 「Azure仮想ネットワーク」: windows1_group-vnet-asr
- 「OK」
- Recovery Servicesコンテナー＞監視＞Site Recoveryジョブ
  - テストフェールオーバーの状態: 「テストフェールオーバーの初期化中」、しばらくすると「成功」あるいは「テスト フェールオーバーのクリーンアップが保留中」となる

テストフェールオーバーによって作成されたVMの動作確認:

- 仮想ネットワーク windows1_group-vnet-asr 以下（リソースグループ win1_group-asr 以下）に作成されたVMを確認
  - VM、ディスク、NICが作成される
  - 名前の末尾は「test」となっている
- IPアドレスやNSGは作成されないので、手動で付与し、RDP接続（またはBastion経由で接続）

テストフェールオーバーのクリーンアップ:

- Recovery Servicesコンテナー＞保護されたアイテム＞レプリケートされたアイテム＞windows1
- 「テストフェールオーバーのクリーンアップ」
- 「テストが完了しました。テスト フェールオーバー仮想マシンを削除してください。」にチェック
- 「OK」