# 启动器更新

用户入口：设置 → 其他 → 启动器发布源，填写 `作者/仓库` 或 GitHub 仓库地址，然后点击「检查更新」。没有配置源时，启动时静默跳过，手动检查会提示配置。

更新检查使用该仓库 `/releases/latest`，仅支持公开仓库的最新正式版，不会检查 PCL 官方仓库。版本比较使用启动器程序集版本，支持三段或四段数字（缺省修订号视为 0），不降级。启动检查会遵循自动下载、仅提示、仅手动检查的设置；网络失败不会影响启动。

## 发布新版本

1. 同步修改 `My Project/AssemblyInfo.vb` 的 `AssemblyVersion`、`AssemblyFileVersion` 和 `Modules/Base/ModBase.vb` 的 `VersionBaseName`。当前基线是 `2.13.1.1`，下个版本可使用 `2.13.1.2`。
2. 使用 `Harness.BuildOnly.vbproj` 编译 Release 到 `bin`。
3. 执行 `powershell.exe -NoProfile -ExecutionPolicy Bypass -File output/package-launcher-update.ps1`，生成仅含启动器 EXE、同目录 DLL 和运行时配置的 `DSHL-update.zip`。不会包含用户设置、日志、缓存或密钥。
4. 在所配置仓库发布正式 Release，标签使用与程序集一致的版本，例如 `v2.13.1.2`，上传该 ZIP。GitHub API 必须提供此附件的 `sha256:` digest；缺少校验值时拒绝下载应用。更新源暂不支持私有仓库登录。

发布包必须是扁平目录，包含 `PCL-Deepseek-Harness-Launcher.exe` 及其所需 DLL；程序身份和版本必须与 Release 对应。不能将源码 ZIP 当作更新包。用户配置的仓库是更新信任来源，校验值验证的是文件完整性，不代替发布者身份判断。

## 应用与恢复

下载通过任务管理器显示进度，可取消、失败后重试。下载完成后校验长度、SHA256、ZIP 路径、解压大小、程序集身份及版本，随后询问立即重启或退出时更新。其他下载任务未完成时暂缓立即重启。

独立 PowerShell 更新器等待旧进程退出，逐个备份并替换发布文件；替换失败尝试回滚。只处理包内的程序文件，不清理用户目录。更新过程和备份位于启动器目录下 `PCL/LauncherUpdate/<任务编号>/`，`result.txt` 表示成功，`error.txt` 记录失败及可能的回滚错误；下次启动显示结果。保留备份供人工恢复，不自动删除。

断电、系统强制结束更新器不保证自动回滚，可从相应 `backup` 子目录恢复。需要启动器目录写权限。下载完成但应用前启动器异常退出时，可重新检查下载更新。

## 验证

先编译至 `bin/UpdateReview/`，运行 `output/test-launcher-update.ps1`。测试覆盖版本比较、发布解析、预发布过滤、损坏包、ZIP 路径穿越、隔离目录替换、文件占用回滚、应用前篡改校验和用户设置保留。测试不替换正在使用的启动器。
