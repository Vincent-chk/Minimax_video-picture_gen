# MiniMax 本地生成工具

这是一个基于 PyQt6 的本地桌面 GUI，用来调用 MiniMax 的视频、图片、音乐和语音生成 API。

在 [MiniMax 开放平台](https://platform.minimaxi.com) 购买 Token Plan 套餐后，即可使用本工作台进行可视化的图片生成与视频生成。无需编写代码，通过图形界面即可完成从 prompt 编写到素材预览的完整创作流程。
注意：只适用token plan的API（中档次及以上才有视频生成额度）或者minimax的普通API，如果用的是media plan就直接用minimax hub即可。

## 快速开始

推荐使用 `uv`，这样可以直接复用仓库内的 `uv.lock`，减少依赖版本差异。

```bash
git clone https://github.com/Vincent-chk/Minimax_video-picture_gen.git
cd minimax_video_gen
uv sync
uv run minimax-local-generator
```

启动后输入你的 MiniMax API Key。应用会先做一次极简调用校验；校验通过后，API Key 只保存在本次运行内存中，关闭程序后不会写入本地文件。

## 功能

- 启动时输入 MiniMax API Key，确认时先用 `MiniMax-M2.7` 做极简调用校验；通过后本次运行内锁定，不写入本地文件。
- 视频生成：文生视频、图生视频、首尾帧、主体参考。
- 图片生成：文生图、参考图生成。
- 音乐生成：原创音乐、画面 BGM、歌词生成、一键翻唱、两步翻唱。
- 语音生成：音色快速复刻、同步语音合成、异步语音合成，并支持刷新可用音色列表。
- 视频、图片、音乐和歌词 prompt 支持提交前自动优化与回退。
- 本地素材会做格式和大小校验；成功生成的文件保存到 `outputs/` 下对应目录。
- 生成后可在本地 GUI 中预览图片、播放视频/音频，并打开输出目录。

## 推荐环境

- Python `>=3.9`
- 推荐使用 `uv` 管理依赖和虚拟环境
- macOS、Windows 和 Linux 均可运行；桌面显示依赖 PyQt6，本机需要有可用的图形界面环境。

安装 `uv` 可参考官方文档：https://docs.astral.sh/uv/

## 安装与运行

首次 clone 后执行：

```bash
uv sync
```

启动应用：

```bash
uv run minimax-local-generator
```

也可以继续使用主入口：

```bash
uv run python main.py
```

## API Key

- 你需要准备一个可用的 MiniMax API Key。
- API Key 在启动应用时输入，不需要写入 `.env`、配置文件或源码。
- 本项目不会持久化保存 API Key；每次重新打开应用都需要重新输入。
- 校验 API Key 时会调用 MiniMax API。后续生成视频、图片、音乐或语音时，以 MiniMax 平台实际计费规则为准。

## 测试

```bash
uv run python -m unittest
```

测试会 mock MiniMax API，不会真实提交生成任务或扣费。

## pip 兼容方式

如果不使用 `uv`，也可以使用 pip：

```bash
python3 -m pip install -r requirements.txt
python3 main.py
```

但推荐优先使用 `uv`，因为 `uv.lock` 可以让其他用户获得更稳定、可复现的依赖版本。

## 输出文件

生成成功的文件会保存到本地 `outputs/` 目录。该目录已加入 `.gitignore`，避免把本地生成的视频、图片、音频或歌词误提交到 GitHub。

## 常见问题

### 启动时报 PyQt6 相关错误怎么办？

先确认已完成依赖安装：

```bash
uv sync
```

如果使用 pip，请确认当前 Python 环境中已安装 `PyQt6`：

```bash
python3 -m pip install -r requirements.txt
```

Linux 用户如果在无桌面环境的服务器上运行，可能需要先配置图形界面或改在本地桌面系统运行。

### API Key 校验失败怎么办？

请确认 API Key 没有多余空格、仍然有效，并且当前网络可以访问 MiniMax API。应用固定使用 `https://api.minimaxi.com/v1`。

### 测试会真实调用 MiniMax 或扣费吗？

不会。测试会 mock MiniMax API，不会真实提交生成任务，也不会产生生成费用。

### 生成结果在哪里？

默认在项目目录下的 `outputs/` 中，并按图片、音乐等类型保存。你也可以在 GUI 中直接打开输出目录。

## 注意

- Base URL 固定为 `https://api.minimaxi.com/v1`。
- WebSocket 语音同步合成使用 `wss://api.minimaxi.com/ws/v1/t2a_v2`。
- 本地 GUI 不实现公网 `callback_url`；需要查询的任务统一使用轮询。
- API Key 只保存在本次运行内存中；关闭程序后需要重新输入。

## 许可证

本项目使用 MIT License。详见 `LICENSE`。
