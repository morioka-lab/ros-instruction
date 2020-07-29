# machine learning
machine_learning/deep_reinforcement_learning/DDQN/scripts/settings.py これがDQNの設定ファイル

TRAINで学習した結果を再生するのか学習するのかをTrue/Falseで設定する
LOAD_NETWORK_PATH


ENV_PATHに環境の設定。
https://github.com/morioka-lab/machine_learning/tree/master/deep_reinforcement_learning/DDQN/environments/nakano_11f

MEMORY_SIZE = 150000
BATCH_SIZE = 32
150000の過去のデータから32こ取ってきて学習する

まずはランダムデータを貯める。40000とか適当に溜まったら学習開始する。

https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/agent.py#L106

`#train`にぶちこめば学習できる
https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/dqn.py


python main.py で適当に実行される


SAVED_NETWORK_PATH = "./../saved_network"に学習結果が保存される

summaryは便利。tensorboard。https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/settings.py#L22 に置かれる。

学習後のファイル名はこれ
https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/agent.py#L133

epsilonはランダム行動に関する値。
https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/settings.py#L6

ロボットが取りうる行動。
ACTION_MAP= [[直進, 回転], [][][]]
https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/settings.py#L26

報酬の設定はここ
https://github.com/morioka-lab/machine_learning/blob/master/deep_reinforcement_learning/DDQN/scripts/environment.py



実際にロボットを動かすにはこれを編集する
dqn_navigation/config/nakano_11f_1.json
ぱいせんは適当にリネームして管理している


実行前に`venv_tf`が必要。tensorflowをアクティベートする。
ぱいせん独自のエイリアス

`. ~/python/venv/tf_r1.6/bin/activate`

