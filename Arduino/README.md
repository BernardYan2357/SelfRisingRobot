# robo03

这是一个使用 M5Atom 控制二自由度机器人，并通过预训练策略执行起身动作的 Arduino 草图。

## 文件

- `robo03/robo03.ino` - 用于 M5Atom 的控制草图
- `robo03/policy_network.h` - 将预训练策略转换为 C 头文件后的文件

`policy_network.h` 会被 `robo03.ino` 读取，因此必须放在同一个 `robo03` 文件夹中。

## 所需库

请通过 Arduino IDE 的库管理器等方式安装以下库：

- M5Atom
- Kalman
- ESP32Servo

此外还需要 ESP32 开发板环境。

## 上传

在 Arduino IDE 中打开 `robo03/robo03.ino`，选择 M5Atom 对应的开发板进行编译并烧录。

## Wi-Fi 控制

启动后，M5Atom 会创建 Wi-Fi 接入点。

- SSID: `robo1`
- Password: `password`
- URL: `http://192.168.42.1`

通过浏览器访问该地址，可以手动调整舵机、开始/停止起身动作，以及开启/关闭自动起身。

## 按钮

也可以通过 M5Atom 本体上的按钮开始起身动作。

起身过程中按下按钮，会停止起身动作并返回手动模式。

## 注意事项

- 假定舵机 1 连接到 GPIO 26，舵机 2 连接到 GPIO 32。
- 舵机偏移量和脉冲宽度可以通过网页进行调整，并保存到 Preferences 中。
- `policy_network.h` 是已生成的策略网络。如果要使用其他训练结果，请替换此文件。
