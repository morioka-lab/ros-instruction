# ロボットを動かす
## 1. ジョイスティックでロボットを動かす
ジョイスティックでロボットを動かします。1で立ち上げたroscoreとrosrunはCtrl+Cで切っておきましょう。

```
$ roslaunch icart_navigation waypoint_recorder.launch
```

これだけでロボットが動くようになるはずです。操作方法は、oが前進□が後進、スティックで左右に曲がります。

### (おまけ) 別の地図で動かすには
デフォルトの地図は中野キャンパス11Fになっているのですが、別の地図を使いたいときは森岡研のGitHubからダウンロードしたros/の中にある地図の設定を書き換えます。

```
$ roscd icart_navigation
$ vim launch/map.launch
```

6行目の`<node pkg="map_server" type="map_server" name="map_server" args="/home/<USERNAME>/my_workspace/src/maps/nakano/nakano_11f.yaml" />`が地図の設定になります。ここを書き換えることにより別の場所の地図をロードできます。

## (おまけ) パラメータの設定
ロボットを動かすにはロボットごとに適切なパラメータをセットしてやる必要があります。

パラーメータのファイルは`~/my_workspace/src/ypspur_ros/params`に入っています。

```shell
$ ls ypspur_ros/params
icart-middle.param  otake.param  suzuki.param  yuda.param
```

こんな感じでここに`<自分の名前>.param`でファイルを設定するのがよいでしょう。

このパラメータファイルは`~/my_workspace/src/ypspur_ros/launch/ypspur_ros.launch`で実際に設定されています。

```shell
$ git grep suzuki.param
ypspur_ros/launch/ypspur_ros.launch:    <arg name="param" default="$(find ypspur_ros)/params/suzuki.param"/>
```

## (おまけ) センサーに繋げない
以下のようなエラーが出ることがあります。

```
Device Information
 Port    : /dev/serial/by-id/usb-T-frog_project_T-frog_Driver-if00
Error: Can't open serial port.
```

こんな時は

```
sudo chmod 666 /dev/ttyACM0 && sudo chmod 666 /dev/ttyACM1
```

を試してみてください。それでもだめなら


* USBケーブルを抜き差し
* USBハブを取ってPCの直接挿す
* ロボットの再起動
* バッテリーの充電をする

を見てみましょう。
