<h1 align="center">DSHL · DeepSeek Harness Launcher</h1>

<p align="center">
  <b>在 Windows 上一站式安装、启动与管理 DeepSeek Harness 的桌面启动器</b>
</p>

<p align="center">
  <a href="https://dsh-packforge.github.io/dsh-pack-market/"><img src="https://dsh-packforge.github.io/dsh-pack-market/badges/launchers/dsh-packforge-support-zh.svg" alt="支持 DSH-PackForge 整合包"></a>
  <img src="https://dsh-packforge.github.io/dsh-pack-market/badges/versions/manifest-v5-zh.svg" alt="清单 v5">
  <img src="https://dsh-packforge.github.io/dsh-pack-market/badges/versions/pack-v3-zh.svg" alt="结构 v3">
</p>

---

## 📥 下载安装

前往 [**Releases**](https://github.com/Loliyer520/DSHL-Deepseek-Harness-Launcher/releases) 下载最新版本。

- 运行环境：Windows 10/11 + WSL2
- 启动器会自动检测并一键安装所需环境（WSL、Node.js、dsh），无需手动配置

## ✨ 功能特性

- **🚀 Harness 启动管理**：通过 WSL2 拉起 dsh CLI，支持 `web` / `tui` / `headless` 三种 profile，多实例并行，崩溃自动救援
- **📦 一键环境安装**：自动检测 WSL、Node.js（`≥22.18`（22 线）或 `≥24.2`（24 线））与 dsh，缺失组件一键补齐
- **🔀 版本库**：`~/.dsh-versions` 多版本并存，随时切换「当前启动版本」
- **🧩 插件社区**：浏览、安装、更新、卸载 dsh 插件（`topic:dsh-plugin`）
- **🎒 整合包**：`.dspack` 导入导出，在 [整合包市场](https://dsh-packforge.github.io/dsh-pack-market/) 发现更多玩法
- **🤖 内置助手「卡西」**：AI 对话 + shell 工具循环，报错可一键「用 Agent 分析」诊断
- **🔄 自动更新**：启动器自更新，SHA256 校验，始终用上最新版

## 与 PCL 的关系

本项目属于对 PCL 的「重度使用」二次创作，遵循 PCL 分发许可与本仓库 `LICENCE` 文件中的《PCL 存储库合理使用指南》。

反馈渠道：

- **Bug 报告** → [Issues](https://github.com/Loliyer520/DSHL-Deepseek-Harness-Launcher/issues/new?template=bug_report.yml)
- **功能建议** → [Issues](https://github.com/Loliyer520/DSHL-Deepseek-Harness-Launcher/issues/new?template=feature_request.yml)

- 本项目名称以 **Plain Craft Launcher (PCL)** 开头并带第三方后缀（`PCL-Deepseek-Harness-Launcher`）。
- 在此向 PCL 原作者 **[龙腾猫跃](https://github.com/Meloong-Git)** 致谢与署名。
- 本仓库为 DSHL 的官方发布与反馈渠道，继续以 `LICENCE` 文件作为使用指南。

---

## 环境要求

**运行：**

- Windows 10 / 11（64 位）
- .NET Framework 4.8
- WSL2（程序可引导一键安装）
- 发行版内 Node.js `^22.19.0` 或 `>=24.0.0`

**构建：**

- Visual Studio 2022（含 .NET 桌面开发工作负载，用于编译 WPF 主工程）
- .NET SDK（用于编译 C# 依赖：`PCLCS`、`MeloongCore`）
- Python 3（用于生成 build-only 工程文件）

---

## 构建

主工程为 VB.NET / WPF（.NET Framework 4.8），另有三个 C# 依赖项目。一条命令即可完成：

```bat
build-harness.bat
```

等价于以下步骤：

```bat
:: 1/3 用 dotnet SDK 编译 C# 依赖
dotnet build PCLCS\PCLCS.csproj -c Debug --nologo -v q
dotnet build MeloongCore\Shared\MeloongCore.csproj -c Debug --nologo -v q
dotnet build MeloongCore\Wpf\MeloongCore.Wpf.csproj -c Debug --nologo -v q

:: 2/3 生成 build-only vbproj（把 ProjectReference 换成 DLL 直接引用）
python gen-buildonly-vbproj.py

:: 3/3 用 VS MSBuild 编译 WPF 主工程
"C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\MSBuild.exe" "Harness.BuildOnly.vbproj" /t:Build /p:Configuration=Debug
```

> 脚本内的绝对路径（`build-harness.bat`、`gen-buildonly-vbproj.py`）是作者本机路径，其他机器构建前需改为实际路径。

---

## 使用说明

1. **安装环境**：首次启动点击 Harness 启动，程序检测到缺少 WSL / Node / dsh 后会引导一键自动安装。
2. **启动 Harness**：在启动页选择 profile（`web` / `tui` / `headless`）与端口（默认 `10721`），点击启动；`web` profile 会从日志解析出带 token 的访问地址。
3. **管理插件**：下载页的插件社区按关键词搜索 GitHub 插件仓库，一键安装到指定 profile；实例页可查看已安装插件并更新 / 卸载。
4. **整合包**：导入 `.dspack` 或含 `distribution.json` 的 ZIP；导出页可把当前 profile 打包为 `.dspack`；下载页的整合包市场（[dsh-pack-market](https://dsh-packforge.github.io/dsh-pack-market/)）可浏览并一键安装。
5. **助手**：助手页配置 API 地址 / Key / 模型后即可对话；报错与日志面板的「用 Agent 分析」会把内容带入助手页诊断。

---

## 反馈与数据收集

<a id="feedback"></a>
### 问题反馈

遇到报错、Bug，或想提功能建议，请到 [Issues](https://github.com/Loliyer520/DSHL-Deepseek-Harness-Launcher/issues) 提交；程序崩溃时写入的日志也会提示该地址。

<a id="telemetry"></a>
### 匿名数据收集（遥测）

当下载失败、启动器出错、程序运行异常等事件发生时，启动器会**匿名**上报信息，以通知开发者「哦吼，出错了」，从而更好地修 Bug。上报内容**不含任何个人信息**，也不会收集。可在 设置 → 其他 → 启动器 中关闭上报。

<a id="homepage-submission"></a>
### 联网主页投稿

如需制作联网更新的主页（服主可用于动态更新服务器公告），可在设置的主页自定义里点击提示投稿，若合格即可加入预设。制作联网主页时，可通过版本号检查节省流量，也可通过检查 Referer 和 User Agent 判断对方的 PCL 版本。

---

## 发布新版本

启动器自更新的发布流程见 [UPDATING.md](UPDATING.md)：同步版本号 → Release 编译 → 生成 `DSHL-update.zip` → 发布 GitHub Release（附 `sha256:` 校验值）。

---

## 目录结构

```
PCLCS/                  C# 依赖：启动/Java/资源解析等
MeloongCore/            C# 依赖：Shared / Wpf 核心库
Modules/                核心逻辑（VB.NET）
  ├─ Base/              基础模块（日志、加载器等）
  ├─ Minecraft/         PCL 原有 Minecraft 模块
  └─ DeepSeek/          DSHL 专属模块
       ├─ ModHarness.vb       dsh 进程启动/监视
       ├─ ModWsl.vb           WSL 环境检测与安装
       ├─ ModAssistant.vb     内置助手（Agent 对话）
       ├─ ModPluginCommunity.vb  插件社区/管理
       ├─ ModDspack.vb        .dspack 整合包
       └─ ModLauncherUpdate.vb    启动器自更新
Pages/                  页面（启动/下载/实例/助手/设置等）
Controls/               自定义控件
FormMain.xaml(.vb)      主窗口
```

---

## 🔗 相关资源

- 整合包市场（网页）：[dsh-packforge.github.io/dsh-pack-market](https://dsh-packforge.github.io/dsh-pack-market/)
- 市场仓库：[DSH-PackForge/dsh-pack-market](https://github.com/DSH-PackForge/dsh-pack-market)
- 格式规范：[DSH-PackForge](https://github.com/DSH-PackForge/DSH-PackForge)
- 环境库：[T-Auto/dsh-distribution](https://github.com/T-Auto/dsh-distribution/)

## 🎯 开源目标

为了避免生态环境一直处于**重复造轮**、**协议混乱**、百家争鸣的状态，DSHL 致力于建立一个结合当今主流插件与整合包协议的社区，以最便捷的途径为**开发者**与**普通用户**群体提供帮助。

项目开源计划将在 DSHL 深度整合包完成发布并开源后逐步落地。

## 📜 关于源码

DSHL 基于 [Plain Craft Launcher 2](https://github.com/Meloong-Git/PCL) 二次开发，遵循其分发许可与《PCL 存储库合理使用指南》。

本仓库是 DSHL 的**官方发布页与用户反馈渠道**。源码开放节奏见上方「开源目标」；在那之前，你遇到的每一个问题、提的每一条建议，都会直达开发桌面。

## ⚖️ 许可

DSHL 二进制版本可免费下载、使用与分发（不得用于商业用途），源码保留所有权利。详见 [LICENSE](LICENSE)。

## 免责声明

本项目是个人学习/二次创作项目，与 DeepSeek 官方无任何关联；`DeepSeek`、`DeepSeek Harness` 等为各自所有者的商标。使用过程中涉及的网络请求、插件安装与 dsh 运行请遵守相关服务条款与所在地法律法规。
