# ROSセットアップ
森岡研で使用するロボットを0から動かすための手順書です。

## 事前に必要なもの
* Ubuntu 16.04.05 Xenial
* **GitHubアカウント**とmorioka-labへの招待

### morioka-labへの招待
morioka-labに入りたかったらSlackの「ホームページ更新」チャンネルで「@moriokalabcombot invite github <GitHubに登録したメールアドレス>」と入力しましょう。招待メールが届くはずです。

![invite-github](/docs/images/invite-github.png)

よく分からなかったら森岡先生に聞いてみましょう。

## 簡単セットアップ
以下のコマンドを実行すれば必要なライブラリのインストール、初期ワークスペースの作成が一発で行えます(たぶん)。

```shell
$ curl -sL --proto-redir -all,https https://raw.githubusercontent.com/morioka-lab/ros-instruction/master/setup-melodic.sh | bash
```

たまにパスワードとか求められるので見ておきましょう。

何か失敗した場合は[セットアップ手順](/docs/instruction.md)を見て手でやってみましょう。上のスクリプトはこの資料の中に書いてあることを自動で実行します。

## ロボットを動かす
以下の順番で作業をしてください。

1. [はじめに](/docs/get_started.md)
2. [ロボットを動かす](/docs/move_robot.md)
3. [ロボットの地図に関して](/docs/map.md)
3. [地図作成](/docs/map-creation.md)
4. [ロボットの自律移動](/docs/autonomous_mobile.md)
5. [GPUを使った学習の環境構築](/docs/setup_for_machine_learning.md)
