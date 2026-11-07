# BA MomoTalk Auto (Steam / PC 版)

基于 [SmileMint/ba-momotalk-auto](https://github.com/SmileMint/ba-momotalk-auto) 修改，适配 **Blue Archive 官方 Steam / PC 客户端**。

原版通过 MuMu 模拟器的 ADB 截图和点击；本版本改为：

- 用 `mss` 截取游戏窗口画面
- 用 `pyautogui` 模拟鼠标点击 / 拖动
- 用 `pygetwindow` 自动查找游戏窗口

图像识别逻辑（颜色阈值找红点、回复按钮、剧情入口等）基本保持原样。

## 安装依赖

```bash
pip install -r requirements_steam.txt
```

## 游戏推荐设置

1. **窗口模式**（推荐），分辨率 **16:9**（1280×720 或 1920×1080 最佳）
2. 游戏窗口完全显示在主屏幕内，不要被挡住
3. 运行脚本时不要移动鼠标、不要切换窗口（脚本会占用鼠标）
4. 进入主界面（能点左上角 MomoTalk 的界面）后再启动脚本

## 运行

```bash
# 先启动 Steam 版 Blue Archive，进入主界面
python ba_momotalk_auto_steam.py
```

常用参数：

```bash
# 指定窗口标题（找不到窗口时用）
python ba_momotalk_auto_steam.py --window-title "Blue Archive"

# 只截图判断，不实际点击（调试用）
python ba_momotalk_auto_steam.py --dry-run

# 自检：截一张图并打印识别结果
python ba_momotalk_auto_steam.py --self-test

# 限制次数
python ba_momotalk_auto_steam.py --max-rewards 50 --max-steps 500
```

紧急停止：把鼠标快速移到屏幕**左上角**（pyautogui 的 FAILSAFE），或按 `Ctrl+C`。

## 和原版的主要差异

| 项目 | 原版 (MuMu) | Steam 版 |
|------|-------------|----------|
| 截图 | ADB `screencap` | `mss` 截游戏窗口 |
| 点击 | ADB `input tap` | `pyautogui.click` |
| 返回键 | Android BACK | `ESC` 键 |
| 连接 | `--adb` / `--serial` | `--window-title` |

## 注意事项

1. **UI 缩放 / 边框**：如果窗口带标题栏，实际游戏画面可能有偏移。若点击位置不准，可先 `--self-test` 看截图是否完整包含游戏画面，必要时用无屏或无边框窗口。
2. **分辨率**：脚本按 1920×1080 基准做比例缩放，其他 16:9 分辨率一般可用；非 16:9 可能识别失败。
3. **剧情跳过**：PC 端用 ESC 调出菜单再点确认，若你的客户端按键不同，可能需要改 `back()` 方法。
4. **风险**：自动化可能违反服务条款，账号风险自负。本脚本只处理 MomoTalk 和羁绊剧情奖励，不做抽卡/购买。

## 调试

卡住时会在 `ba_momotalk_debug/` 下保存 `stopped.png` 等截图，方便对照识别区域。

## License

原项目 MIT License。本修改同样可按 MIT 使用。
