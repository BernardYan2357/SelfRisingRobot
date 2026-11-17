# robo1 起身 PPO

这是用于让 MuJoCo 中的二自由度机器人 `robo1` 从倒地姿态起身的预训练 PPO 模型。

## 文件

运行此仓库所需的文件如下：

- `robo1_getup_ppo.zip` - Stable-Baselines3 PPO 预训练模型
- `robo1_env.py` - Gymnasium/MuJoCo 环境
- `robo1.xml` - MuJoCo 模型定义
- `assets/` - `robo1.xml` 引用的 STL 网格文件
- `play_robo1_policy.py` - 回放预训练模型
- `eval_robo1_policy.py` - 从各种倒地姿态进行评估
- `search_all_getup.py` - 搜索各倒地姿态的起身轨迹候选，并输出用于更新 `getup_reference.py` 的 `BEST_SEQ`
- `getup_reference.py` - 用于生成教师数据的各倒地姿态舵机目标角 waypoint 序列
- `scripted_getup.py` - 在 MuJoCo viewer 中回放并确认 `getup_reference.py` 定义的起身轨迹
- `pretrain_robo1_from_scripted.py` - 根据 `getup_reference.py` 中的轨迹生成观测与教师 action 对，并预训练 PPO 策略
- `train_robo1.py` - 使用 PPO 进行追加训练
- `export_policy_header.py` - 根据预训练模型生成 Arduino C 头文件
- `requirements.txt` - Python 依赖包

## 设置

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## 回放

```powershell
python play_robo1_policy.py
```

从特定初始姿态开始回放时：

```powershell
python play_robo1_policy.py --pose roll_pos
python play_robo1_policy.py --pose roll_neg
python play_robo1_policy.py --pose pitch_pos
python play_robo1_policy.py --pose pitch_neg
```

## 评估

```powershell
python eval_robo1_policy.py
```

## 训练

根据 `getup_reference.py` 中定义的起身教师数据预训练初始策略。

如果需要重新搜索起身轨迹，请执行：

```powershell
python search_all_getup.py
```
将输出的 `BEST_SEQ` 或 `REFERENCE_CANDIDATES` 数组更新到
`getup_reference.py` 的 `getup_sequence_for_pose()` 中。


根据教师数据预训练初始策略

```powershell
python pretrain_robo1_from_scripted.py
```

此操作会生成 `robo1_getup_ppo.zip`。

接下来使用 PPO 进行追加训练。

```powershell
python train_robo1.py --model-in robo1_getup_ppo.zip --timesteps 200000 --n-envs 6 --model-out robo1_getup_ppo
```

如果要从头开始 PPO 训练，请省略 `--model-in`。

```powershell
python train_robo1.py --timesteps 200000 --n-envs 6 --model-out robo1_getup_ppo
```

## 导出到 Arduino

根据预训练模型生成 `policy_network.h`。

```powershell
python export_policy_header.py robo1_getup_ppo.zip -o policy_network.h
```

将生成的 `policy_network.h` 放置到 Arduino 草图目录中即可使用。

## 注意事项

- `robo1_getup_ppo.zip` 依赖于 `robo1.xml` 和 `robo1_env.py` 中的环境定义。
- 如果缺少 `assets/` 中的 STL 文件，则无法加载 MuJoCo 模型。
- 请在仓库根目录下执行命令。
