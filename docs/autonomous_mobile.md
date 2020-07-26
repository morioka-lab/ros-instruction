# ロボットの自律移動
## 1. ウェイポイントの記録
ウェイポイントとはゴールまでの中継地点のことです。ロボットは自律移動するとき全ての中継地点を正しい順番で通らなければいけません。

まずは`waypoint_recorder`を立ち上げます。

```shell
$ roslaunch icart_navigation waypoint_recorder.launch
```

次に別のターミナルでrvizを起動しましょう。

```shell
$ roslaunch my_utility rviz.launch
```

画面はShift + ドラッグで移動できます。

ではここでロボットを研究室の外に出しましょう。ロボットを研究室の外に出したらrvizを起動してロボットの初期位置を手で設定します。

メニュー中にある**2D Pose Estimate**を押して出てくる矢印を研究室前で進行方向に向かってドラッグ&ドロップします。進むうちに場所は修正されるので結構適当でいいです。

![ros-map-2](/docs/images/ros-map-3.png)

赤い線がセンサーが取得した壁です。これがマップ上の実際の壁と同じくらいの位置に来るようにしてください。

こんな感じになります。

![ros-map-4](/docs/images/ros-map-4.png)

次に実際にウェイポイントを記録していきます。別のターミナルで以下のコマンドを実行してください。

```shell
$ cd ~/my_workspace
$ rosrun goal_server goal_saver -f nakano_11f.csv
```

ロボットを動かしながらウェイポイントを設定していきます。中継地点にしたい場所でコントローラーの右スティックを押してください。`rosrun goal_server`の画面に表示が出るはずです。

中継地点は各曲がり角で設定します。

設定が終わったらCtrl + Cで抜けます。これで保存が完了します。

こんな感じの出力が見えればOKです。

![ros-goal_saver](/docs/images/ros-goal_saver.png)

(おまけ) ロボットを動かさなくてもrviz上でウェイポイントを保存できる仕組みを先輩が作ってくれています: [画面クリックからウェイポイントを取得する](/docs/way_points_from_rviz.md)

## 2. ウェイポイントを使った自律移動
上で設定したウェイポイントに沿ってロボットを自律移動させてみます。

まずは以下のコマンドを実行してください。

```
$ roslaunch icart_navigation icart_navigation.launch
```

次に、rvizを開いて2D Pose Estimateで初期位置を設定します。

そして最後に以下のコマンドで`goal_queue`にウェイポイントを追加したらロボットが動き出します。

```
$ rosrun goal_server goal_server nakano_11f.csv
```
