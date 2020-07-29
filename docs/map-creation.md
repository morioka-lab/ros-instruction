# gmappingによる地図作成
ロボット走行用の11階の地図を作成します。

いつも読み込んでいるマップ([ロボットの地図に関して](/docs/map.md))の実態はただの画像です。この画像はレーザーセンサーで読み取った値から作成されるものです。

## 手順
以下の2コマンドを入力するとrviz上に地図が作成され始めます。コントローラーでロボットを動かすと地図が作られていくのが分かると思います。

```bash
roslaunch icart_navigation gmapping.launch
roslaunch my_utility rviz.launch
```

満足いく地図が出来たら保存します。保存方法は以下です。

```
rosrun map_server map_saver -f <ファイル名>
```

## Error: Can't open serial port. / failed to init libypspur が出る
![device-forbidden-error](/docs/images/device-forbidden-error.png)

上のようなエラーが出る場合、以下を実行してください。

```bash
sudo chmod 666 /dev/ttyACM0 && sudo chmod 666 /dev/ttyACM1
```

これだと毎回上記コマンドを打たなければいけないので嫌な人は https://moriokalab.slack.com/archives/C8Z72J4TB/p1595398117005500 のやり取りを見て設定しましょう。

## うわっ…私のロボット、速すぎ…？
速すぎると思ったらロボットのパラメーターをいじりましょう。パラメーターは`ypspur_ros/params/yuda.param`である場合が多いです。違ったら`ypspur_ros/launch/ypspur_ros.launch`の中を確認してみましょう。

そしてパラメーターファイルの`MAX_VEL`を0.4くらいにすれば速すぎるということはなくなるはずです。

## なんかグルグル回る
以下のようにrviz上の様子がおかしい場合があります。

![rviz-guruguru](gmapping-lost.gif)

このようになるのは座標系が反転しているからです。また、以下のようにrvizをよく見ると壁とは反対の部分が壁として認識されています。

![tf-inverted](/docs/images/tf-inverted.png)

これを解決するには`icart_navigation/launch/gmapping.launch`中の`<!-- tf -->`と書かれている部分を編集する必要があります。

```
<!-- tf -->
<node pkg="broadcaster" type="laser_tf_broadcaster" name="laser_tf_broadcaster" />
```

この`type="laser_tf_broadcaster"`を`laser_tf_broadcaster`なら`inverted_laser_tf_broadcaster`に、`inverted_laser_tf_broadcaster`なら`laser_tf_broadcaster`に変えます。

こうすることでセンサーの座標系が反転して正しく認識されるようになります。

> 補足
> 座標系はHokuyoのレーザーセンサーの向きによって決まります。レーザーセンサーが上を向いているものは`laser_tf_broadcaster`で、下を向いているものは`inverted_laser_tf_broadcaster`を使います。
