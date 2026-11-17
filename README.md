# SelfRisingRobot

复刻 https://github.com/homemadegarbage/SelfRisingRobot 的 SelfRisingRobot

## 目录

- `3Dmodel/` - 3D 打印模型
- `RL/` - MuJoCo 模型、强化学习环境和训练好的 PPO 模型
- `Arduino/` - M5Atom 用程序和导出的策略网络

各文件夹中的详细使用方法请参阅相应的 README

## 3D 模型

`3Dmodel/` 中包含用于制作实机的 STL 文件。

- `footP.stl`
- `arm1P.stl`
- `arm2P.stl`
- `armhornP.stl`

## 强化学习

`RL/` 中包含仿真环境和训练好的模型。

主要内容：

- MuJoCo 模型
- Gymnasium 环境
- Stable-Baselines3 PPO 训练好的模型
- 回放和评估脚本

## Arduino

`Arduino/` 中包含用于控制实机的 M5Atom 程序。

主要内容：

- `robo03.ino`
- `policy_network.h`

`policy_network.h` 是将训练好的策略转换为 C 头文件后的产物，用于在 M5Atom 上执行起身动作。
