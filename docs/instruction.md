# 環境セットアップ
## 1. 必要な環境のインストール
環境は`Ubuntu 16.04.05 Xenial`を想定しています。残念ながら他の環境だと動かないことが結構あります。

### 1.1 ROSのインストール
ROSに関しては、下の公式ドキュメントに従えばインストールできます。インストールするのはROSのkineticという名前のバージョンです。

http://wiki.ros.org/ja/kinetic/Installation/Ubuntu

### 1.2 yp-spurのインストール
yp-spurとは筑波大学のどこかの研究室が公開している、モーターを動かすためのドライバらしいです。

以下のURLにあるインストールガイドに従ってインストールしましょう。ちなみに`git clone`してくる場所はどこでもいいです。とりあえず私は`~/`で作業しました。

https://www.roboken.iit.tsukuba.ac.jp/platform/wiki/yp-spur/how-to-install

## 2. ROSのワークスペースをセットアップする
### 2.1 ワークスペースの作成
まずはワークスペースのディレクトリを作成します。

```
$ mkdir -p my_workspace/src
```

次に`catkin_init_workspace`でワークスペースを初期化します。

```sh
$ cd my_workspace/src
$ catkin_init_workspace
Creating symlink "/home/asmsuechan/my_workspace/src/CMakeLists.txt" pointing to "/opt/ros/kinetic/share/catkin/cmake/toplevel.cmake"
```

### 2.2 森岡研の独自設定をダウンロードする
森岡研にある自律移動ロボットを動かすための設定等を以下の流れでダウンロードします。

1. GitHubに登録する
2. [森岡研のGitHub Organization](https://github.com/morioka-lab)への招待をもらう(下記参照)
3. ~/my_workspace/srcでgit clone https://github.com/morioka-lab/rosを実行する

morioka-labに入りたかったらSlackの「ホームページ更新」チャンネルで「@moriokalabcombot invite github <GitHubに登録したメールアドレス>」と入力しましょう。招待メールが届くはずです。

![invite-github](/docs/images/invite-github.png)

よく分からなかったら森岡先生に聞いてみましょう。

https://github.com/morioka-lab/ros が見えなかったら権限がありません。git/GitHub周りは聞いてください。

```
$ git clone https://github.com/morioka-lab/ros ~/ros
$ cp -rf ~/ros/* ~/my_workspace/src/
```

### 2.3 必要なROSパッケージをインストールする

```
$ sudo apt install -y ros-kinetic-move-base-msgs ros-kinetic-amcl ros-kinetic-gmapping ros-kinetic-joy ros-kinetic-move-base ros-kinetic-urg-node ros-kinetic-ypspur-ros ros-kinetic-map-server
```

### 2.4 catkin_make
次にワークスペースのルートディレクトリに移動して`catkin_make`します。

```sh
$ cd ..
$ catkin_make
Base path: /home/asmsuechan/my_workspace
Source space: /home/asmsuechan/my_workspace/src
Build space: /home/asmsuechan/my_workspace/build
(中略)
[ 89%] Built target ypspur_ros_gencpp
Scanning dependencies of target joint_position_to_joint_trajectory
Scanning dependencies of target ypspur_ros
[ 91%] Building CXX object ros/ypspur_ros/CMakeFiles/joint_position_to_joint_trajectory.dir/src/joint_position_to_joint_trajectory.cpp.o
[ 93%] Building CXX object ros/ypspur_ros/CMakeFiles/ypspur_ros.dir/src/ypspur_ros.cpp.o
[ 93%] Built target ypspur_ros_generate_messages_eus
Scanning dependencies of target ypspur_ros_generate_messages
[ 93%] Built target ypspur_ros_generate_messages
[ 95%] Linking CXX executable /home/asmsuechan/my_workspace/devel/lib/ypspur_ros/joint_tf_publisher
[ 95%] Built target joint_tf_publisher
[ 97%] Linking CXX executable /home/asmsuechan/my_workspace/devel/lib/ypspur_ros/joint_position_to_joint_trajectory
[ 97%] Built target joint_position_to_joint_trajectory
[100%] Linking CXX executable /home/asmsuechan/my_workspace/devel/lib/ypspur_ros/ypspur_ros
[100%] Built target ypspur_ros
```

#### (おまけ)catkin_makeで何かエラーが出た時
以下のようなエラーが出た時はパッケージが足りていません。必要なパッケージを(ここでは`move_base_msgs`)をaptでインストールしましょう。

ここでポイントは、`sudo apt install ros-kinetic-move-base-msgs`のように`ros-kinetic-<パッケージ名>`であることと、パッケージ名は`_`を`-`に置き換えることです。

```
-- Could not find the required component 'move_base_msgs'. The following CMake error indicates that you either need to install the package with the same name or change your environment so that it can be found.
CMake Error at /opt/ros/kinetic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
  Could not find a package configuration file provided by "move_base_msgs"
  with any of the following names:
```

### 注意事項
普通の人は大丈夫なのですが、以下のような人はそのままじゃ動かないので気をつけましょう。

* bash以外のシェルを使ってる人(zshは大丈夫そうだけどfishはよくわからん)
* GitHubに二段階認証をつけてる人(簡単セットアップでコケるから手でいい感じにして欲しい)

多分この辺の問題で引っかかる人は自分でできる人だと思うので自分で頑張りましょう。
