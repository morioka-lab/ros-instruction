# はじめに
動作確認も兼ねてROSの基礎的な動作に関して少し説明します。

## 0. セットアップ
推奨環境: Ubuntu 18.04 melodic

以下のコマンドを実行すれば必要なライブラリのインストール、初期ワークスペースの作成が一発で行えます(たぶん)。

```shell
$ curl -sL --proto-redir -all,https https://raw.githubusercontent.com/morioka-lab/ros-instruction/master/setup-melodic.sh | bash
```

たまにパスワードとか求められるので見ておきましょう。

## 1. ジョイスティックからの値を取得する
まず`roscore`を立ち上げます。これはrosを動かす上でのサーバーのようなものです。

```sh
$ roscore
... logging to /home/asmsuechan/.ros/log/ac4b97dc-3712-11e9-876a-78929cdc6bd5/roslaunch-asmsuechan-9523.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://asmsuechan:33573/
ros_comm version 1.12.14


SUMMARY
========

PARAMETERS
 * /rosdistro: kinetic
 * /rosversion: 1.12.14

NODES

auto-starting new master
process[master]: started with pid [9568]
ROS_MASTER_URI=http://asmsuechan:11311/

setting /run_id to ac4b97dc-3712-11e9-876a-78929cdc6bd5
process[rosout-1]: started with pid [9636]
started core service [/rosout]
```

### 1.1 ジョイスティックをつなぐ
まずはジョイスティックを繋いでみます。roscoreが立ち上がっていることを確認して、別のターミナルを開きましょう。

```shell
$ cd ~/my_workspace
$ rosrun joy joy_node
[ INFO] [1550892744.686574264]: Opened joystick: /dev/input/js0. deadzone_: 0.050000.
```

こうなれば接続完了です。では実際にジョイスティックからどんな値が得られているかを確認します。また別のターミナルを開きましょう。

```
$ cd ~/my_workspace
$ rostopic echo /joy
header:
  seq: 1
  stamp:
    secs: 1550892777
    nsecs: 616805122
  frame_id: ''
axes: [-0.0, -0.0, 0.0, -0.0, -0.0, 0.0]
buttons: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
---
header:
  seq: 2
  stamp:
    secs: 1550892777
    nsecs: 755331915
  frame_id: ''
axes: [-0.0, -0.0, 0.0, -0.0, -0.0, 0.0]
buttons: [0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
---
```

適当にジョイスティックを動かしてこんな感じの出力が得られればOKです。

## roslaunchで立ち上げる
上の例では`roscore`と`rosrun`を別のターミナルで動かしていました。複雑なシステムになるとこれをいちいちやるのは面倒なので`rosluanch`というものを使って1つのターミナルで済むようにします。

すでにroslaunchで立ち上がるようになっているので、他のroscore/rosrunを止めて以下のコマンドを実行しましょう。

```
$ roslaunch joy_twist joy_twist.launch
```

ちなみに入力中にタブキーを押すと補完してくれて便利です。

ここで`rostopic echo /joy`を実行してジョイスティックを動かした時に何か表示されるなら成功です。
