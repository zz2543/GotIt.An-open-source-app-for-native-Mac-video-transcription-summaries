# 懂听 GotIt

**Mac 原生的播客 / 视频转写与摘要应用。**
把 YouTube、B 站、播客音频链接或本地音频丢进来，懂听会转出完整文字稿，再整理成带章节、要点和原话引用的结构化摘要。

[![Download](https://img.shields.io/github/v/release/zz2543/GotIt.An-open-source-app-for-native-Mac-video-transcription-summaries?label=%E4%B8%8B%E8%BD%BD&logo=apple)](https://github.com/zz2543/GotIt.An-open-source-app-for-native-Mac-video-transcription-summaries/releases/latest)
![Platform](https://img.shields.io/badge/macOS-14%2B-black?logo=apple)
![Arch](https://img.shields.io/badge/Apple%20Silicon-arm64-blue)

[English](#english) · [下载最新版](https://github.com/zz2543/GotIt.An-open-source-app-for-native-Mac-video-transcription-summaries/releases/latest)

---

## 能做什么

- **多种来源**：YouTube、B 站（含 `b23.tv`、`youtu.be` 短链）、音频直链、本地音频文件（拖进窗口即可）。整段分享文案也能直接粘贴。
- **转写 + 结构化摘要**：开场钩子、三幕结构、章节时间线、关键时刻与要点、提到的人物/作品；摘要里的引用会回到原文核对。
- **边听边看**：内置播放器，章节刻度和摘要里的时间戳都能点击跳转。
- **追问**：对单期内容继续提问，基于文字稿回答。
- **一键送总结**：在浏览器里看视频时按全局快捷键（默认 ⌥⌘S），当前标签页直接进入后台队列，完成后系统通知。支持 Safari、Chrome、Arc、Edge；其他 app 会从剪贴板里找链接。
- **分类整理**：手动建分类，也可以让 AI 把「未分类」的视频归进去，你确认后才生效。
- **音频摘要（可选）**：把摘要合成为一段语音。
- **数据在自己手里**：所有处理都在你的 Mac 上调度，用的是你自己的 API；Key 存在系统钥匙串，不落明文。关掉窗口后常驻菜单栏，快捷键照常可用。

## 两种用法

| | Mac 应用 | 网页版 |
|---|---|---|
| 系统 | Apple Silicon Mac，macOS 14+ | **Windows**、macOS（含 Intel）、Linux |
| 安装 | 下载 DMG，拖进「应用程序」 | 装好 Python / Node / FFmpeg 后，一条命令装依赖 |
| 界面 | 原生 SwiftUI 窗口 | 浏览器打开 `http://127.0.0.1:5174` |
| 填 API | 设置里带引导，Key 存钥匙串 | 编辑项目里的 `.env` 文件 |
| 独有功能 | 全局快捷键一键送总结、菜单栏常驻 | — |

两者都是在你自己的电脑上跑同一套后端，用的都是你自己的 API。下面先讲 Mac 应用，[网页版](#网页版windows--macos--linux)在后面。

## 系统要求（Mac 应用）

- Apple Silicon（M 系列芯片）的 Mac，**macOS 14 或更新**。Intel Mac 请用[网页版](#网页版windows--macos--linux)。
- 自己的云端 API（见下文「首次设置」）。懂听不内置任何人的凭据。

## 安装

1. 到 [Releases](https://github.com/zz2543/GotIt.An-open-source-app-for-native-Mac-video-transcription-summaries/releases/latest) 下载 `GotIt-<版本>-arm64.dmg`。
2. 打开 DMG，把 **GotIt** 拖进「应用程序」。
3. **第一次打开会被 macOS 拦下**（"无法验证开发者"或"Apple 无法检查其是否包含恶意软件"）。这是因为懂听没有购买 Apple 开发者签名与公证，不代表文件有问题。放行方法：
   - 先双击打开一次，看到提示后点「完成」；
   - 打开「系统设置 › 隐私与安全性」，往下翻到「安全性」，点懂听旁边的「**仍要打开**」；
   - 输入登录密码确认。之后就能正常双击打开。

   熟悉终端的话，也可以一条命令去掉隔离标记：

   ```bash
   xattr -dr com.apple.quarantine /Applications/GotIt.app
   ```

4. 钥匙串第一次读取 API Key 时 macOS 会问一次，点「**始终允许**」并输入登录密码。

中文系统里 app 显示为「懂听」，英文系统显示为「GotIt」。

## 首次设置

第一次打开会进入「设置 › 接口」，并有一步步的引导：当前要填的字段会高亮，说明去哪里拿、长什么样；关键步骤还能「带我去网站」——打开对应控制台，侧边面板列出每一步点哪里，你在网页上复制的 Key 会被识别并提示填入。

需要准备：

| 用途 | 可选服务 | 说明 |
|---|---|---|
| 摘要与对话（LLM） | 任何 OpenAI 兼容接口（如 DeepSeek）、通义千问、Anthropic | 填地址、模型名、Key，可点「测试连接」 |
| 转写（ASR） | 豆包 / 火山引擎、OpenAI Whisper、Deepgram、通义千问 | 选一家即可 |
| 音频摘要（TTS） | 豆包语音合成、通义千问 | 可选，不需要就关掉 |

填完点「应用并重启后端」。

> **用豆包转写时**：在豆包语音**旧版**控制台创建应用，接入能力勾选 **豆包录音文件识别模型2.0**（不是 1.0 的「录音文件识别大模型」），再勾上录音文件识别的**极速版**——前者转写网上链接里的音频，极速版转写你上传的本地文件。

## 日常使用

- **⌘N** 添加：粘贴链接（整段分享文案也行）或拖入音频文件。
- **⌥⌘S** 快捷提交：浏览器里正在看的视频直接送去总结，可在「设置 › 快捷提交」修改。第一次使用时 macOS 会询问是否允许懂听读取该浏览器，选「好」。
- 关掉主窗口后懂听留在菜单栏，**⌘Q** 才是退出。
- 数据（音频、文稿、数据库）默认在 `~/Library/Application Support/GotIt/data`，可在「设置 › 服务」改位置；日志在 `~/Library/Logs/GotIt/backend.log`。

## 视频下载失败怎么办

YouTube、B 站隔一段时间就会改版，旧版下载组件会失效。遇到大量下载失败时：

**「设置 › 服务 › 解析组件」→「检查并更新」→「重启后端以生效」**

更新装在 app 外面，不影响 app 本身，随时可以「恢复内置版本」。以下情况与版本无关，更新也解决不了：

- 会员专享、年龄限制、地区限制的内容；
- 部分 B 站视频的音频只在中国大陆线路可达，海外网络会失败；
- 使用机房 IP 的代理时，YouTube 成功率明显下降。

## 升级与卸载

- **升级**：下载新的 DMG，把 GotIt 拖进「应用程序」替换即可，数据和设置都会保留。新版本首次打开需按上面的方法再放行一次，钥匙串也会再问一次。
- **卸载**：把 GotIt 移到废纸篓。如需连数据一起清掉，再删除 `~/Library/Application Support/GotIt`、`~/Library/Logs/GotIt`，并在「钥匙串访问」里删除名为 `local.gotit.mac` 的条目。

## 网页版（Windows / macOS / Linux）

网页版在本机跑同一套后端，界面在浏览器里打开，**Windows 也能用**。

### 1. 准备环境

| 组件 | Windows | macOS |
|---|---|---|
| Python 3.11+ | [python.org](https://www.python.org/downloads/) 下载安装，**勾选「Add python.exe to PATH」** | `brew install python` |
| Node.js 20+ | [nodejs.org](https://nodejs.org) 下载 LTS 版 | `brew install node` |
| FFmpeg | `winget install Gyan.FFmpeg` | `brew install ffmpeg` |
| Deno（推荐，YouTube 要用） | `winget install DenoLand.Deno` | `brew install deno` |
| Git | [git-scm.com](https://git-scm.com/download/win) | 系统自带 |

装完**重新打开**终端（PowerShell / 终端），让新装的命令生效。

### 2. 下载源码并安装

```bash
git clone https://github.com/zz2543/Podcast-summary.git
cd Podcast-summary
```

然后**双击启动脚本**：Windows 双击 `start-web.bat`，macOS 双击 `start-web.command`。
第一次会自动装好全部依赖，并用记事本 / 文本编辑打开 `.env` 让你填 API。

也可以用命令行（Windows 把 `python3` 换成 `py`）：

```bash
python3 scripts/web.py install
```

### 3. 填 API

编辑项目根目录的 `.env`，把 `replace-me-*` 换成你自己的值，至少要有：

| 用途 | 要填的项 |
|---|---|
| 摘要（LLM） | `DEEPSEEK_API_KEY`（任何 OpenAI 兼容接口都行，改 `DEEPSEEK_BASE_URL` / `DEEPSEEK_MODEL`）；或 `LLM_PROVIDER=qwen` + `DASHSCOPE_API_KEY`；或 `LLM_PROVIDER=anthropic` + `ANTHROPIC_API_KEY` |
| 转写（ASR） | 默认豆包：`VOLC_ACCESS_KEY_ID`、`VOLC_SECRET_ACCESS_KEY`、`DOUBAO_ASR_APP_ID`、`DOUBAO_ASR_ACCESS_TOKEN`；也可 `ASR_PROVIDER=openai_whisper` / `deepgram` / `qwen` 配对应的 Key |
| 音频摘要（TTS） | 可选；不用就设 `TTS_ENABLED=false` |

豆包应用要开通的能力和 Mac 版一样：**豆包录音文件识别模型2.0** + 录音文件识别**极速版**。`.env` 里每一项上方都有注释说明。

### 4. 启动

再次双击 `start-web.bat` / `start-web.command`（或运行 `python3 scripts/web.py`）。后端在 `8000` 端口、网页在 `5174` 端口启动，浏览器会自动打开 `http://127.0.0.1:5174`。

**关掉这个命令行窗口或按 Ctrl+C，服务就停止。** 数据在项目里的 `data/` 目录。

### 更新网页版

```bash
git pull
python3 scripts/web.py install
```

YouTube / B 站下载大量失败时，同样用这两条命令更新（会一起更新 yt-dlp）。

## 第三方组件

GotIt.app 内附带以下以独立可执行文件形式分发的第三方程序：

- [FFmpeg / FFprobe](https://ffmpeg.org)（GPL v3）：音频转码与封面处理。许可证与源码获取方式见 `GotIt.app/Contents/Resources/licenses/FFmpeg.txt`。
- [Deno](https://deno.com)（MIT）：供视频解析组件处理 YouTube 的网页脚本。
- Python 运行时（PSF License）与 [yt-dlp](https://github.com/yt-dlp/yt-dlp)（Unlicense）等 Python 依赖。

## 源码

应用源码（SwiftUI 客户端 + Python 后端）在 [zz2543/Podcast-summary](https://github.com/zz2543/Podcast-summary)。问题和建议欢迎提 [Issue](https://github.com/zz2543/GotIt.An-open-source-app-for-native-Mac-video-transcription-summaries/issues)。

---

## English

**GotIt** is a native macOS app that turns podcasts and videos — YouTube, Bilibili, direct audio links or local audio files — into full transcripts and structured summaries with chapters, key moments and quotes checked against the transcript. You can play the audio with clickable chapter marks, ask follow-up questions about an episode, sort episodes into categories (optionally with AI), and send the video in your browser's current tab to the queue with a global shortcut (⌥⌘S by default).

Everything runs on your Mac with **your own API keys** (stored in the Keychain). No credentials are bundled.

**Requirements:** Apple Silicon Mac, macOS 14 or later.

**Install**

1. Download `GotIt-<version>-arm64.dmg` from [Releases](https://github.com/zz2543/GotIt.An-open-source-app-for-native-Mac-video-transcription-summaries/releases/latest) and drag GotIt into Applications.
2. The app is not notarized, so macOS blocks the first launch. Open it once, click "Done", then go to **System Settings › Privacy & Security**, scroll to Security and click **Open Anyway** next to GotIt. (Or run `xattr -dr com.apple.quarantine /Applications/GotIt.app`.)
3. When the Keychain asks for access to your API keys, choose **Always Allow**.

**First run:** a guided tour in Settings › Providers walks you through each field and can open the vendor consoles step by step.

| Purpose | Providers |
|---|---|
| Summaries & chat (LLM) | Any OpenAI-compatible endpoint (e.g. DeepSeek), Qwen, Anthropic |
| Transcription (ASR) | Doubao / Volcengine, OpenAI Whisper, Deepgram, Qwen |
| Audio digest (TTS, optional) | Doubao TTS, Qwen |

**Video downloads failing?** Settings › Service › Extraction Component → Check & Update → Restart Backend to Apply.

### Web version (Windows / macOS / Linux)

Runs the same backend locally with the UI in your browser — works on **Windows** and Intel Macs too.

1. Install Python 3.11+ (Windows: tick "Add python.exe to PATH"), Node.js 20+, FFmpeg (`winget install Gyan.FFmpeg` / `brew install ffmpeg`) and, for YouTube, Deno (`winget install DenoLand.Deno` / `brew install deno`).
2. `git clone https://github.com/zz2543/Podcast-summary.git`, then double-click `start-web.bat` (Windows) or `start-web.command` (macOS). The first run installs everything and opens `.env`; or run `python3 scripts/web.py install` (`py` on Windows).
3. Put your own keys in `.env` (LLM: `DEEPSEEK_API_KEY` or any OpenAI-compatible endpoint; ASR: Volcengine/Doubao keys, or Whisper / Deepgram / Qwen; set `TTS_ENABLED=false` if you skip audio digests).
4. Double-click the launcher again (or `python3 scripts/web.py`). The UI opens at `http://127.0.0.1:5174`; closing the window or Ctrl+C stops it.

Update with `git pull` followed by `python3 scripts/web.py install`.

Source code: [zz2543/Podcast-summary](https://github.com/zz2543/Podcast-summary).
