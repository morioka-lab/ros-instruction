# 画面クリックからウェイポイントを取得する
ロボットを実際に動かすのではなく、シミュレーター上でクリックしてウェイポイントを作成する

以下の3行を実行するとマップがrvizが立ち上がる。

```
rosrun goal_server goal_saver_from_rviz
rosrun map_server map_server mapxxx.yml(nakano_11f.ymlとか)
roslaunch my_utility rviz.launch
```

**TODO**
