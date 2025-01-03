# ハンズオン: Azure AD Identity Protection

Identity Protectionの「ユーザーリスクポリシー」を設定し、オンにします。これにより、ユーザーのパスワードがダークウェブ等に流出していることが検出された場合に、ユーザーにパスワードの変更を要求します。

- Azure portal にアクセス https://portal.azure.com/#home
- Azure ADに移動![](images/ss-2022-09-27-08-49-13.png)
- セキュリティに移動![](images/ss-2022-09-27-08-48-10.png)
- Identity Protectionに移動![](images/ss-2022-09-27-08-50-28.png)
- 「ユーザーリスクポリシー」に移動![](images/ss-2022-09-27-08-51-11.png)
- 「制御」の「アクセスのブロック」をクリック![](images/ss-2022-09-27-08-52-03.png)
  - 「アクセスの許可」を選択（「パスワードの変更が必要」にチェック）、「完了」をクリック![](images/ss-2022-09-27-08-52-54.png)
- 「ポリシーを適用する」を「オン」に設定し「保存」をクリック ![](images/ss-2022-09-27-08-53-31.png)

