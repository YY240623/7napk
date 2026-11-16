# 7NKernel Android · 1.5.0 RC

基于用户提供的 `7n.html` 重构的 Android 原生 Jetpack Compose 项目。此版本进入发布候选阶段，优先补齐质量门禁，不宣称已经完成 APK 实机验证。

## 本版本内容
- 原生 Android Activity + Jetpack Compose 页面与动效
- 主页、设备中心、设置页、液态玻璃参数控制台
- 使用 Android 公开 API 展示设备型号、系统版本、ABI 与内核版本
- 通过 PackageManager 启动已安装的指定游戏应用（不注入、不修改游戏）
- SharedPreferences 本地持久化、JSON 导入/导出与外观预设
- 12 个参数分类、243 项参数：8 项联动核心参数，235 项明确标注为预留实验参数
- 将参数目录提取为独立 Kotlin 模块，并新增 10 项 JVM 单元测试声明
- GitHub Actions 增加静态校验、JVM 测试、Android Lint 与 Debug APK 构建步骤

## 本地构建
建议 Android Studio Ladybug 或更新版本，JDK 17、Android SDK Platform 35，以及 Gradle 8.9。

1. 解压项目并用 Android Studio 打开 `7NKernel-Android` 根目录。
2. 等待 Gradle Sync 完成。
3. 运行 `python3 tools/validate_project.py`。
4. 在终端运行 `gradle :app:testDebugUnitTest :app:lintDebug :app:assembleDebug`，或在 Android Studio 中运行对应 Gradle 任务。
5. APK 输出路径：`app/build/outputs/apk/debug/app-debug.apk`。

当前压缩包提供 Gradle Wrapper 的版本配置，但没有 wrapper 启动脚本和 wrapper JAR；请使用 Android Studio 内置 Gradle 集成或本机已安装的 Gradle 8.9。不要把 `gradle-wrapper.properties` 误认为完整 Wrapper。

## GitHub Actions 构建
1. 将解压后的项目目录内容推送到自己的 GitHub 仓库（默认分支 `main` 或 `master`）。
2. 打开仓库的 **Actions**，选择 **Android APK Build and Quality Checks**。
3. 点击 **Run workflow**，等待静态检查、JVM 单元测试、Android Lint 和 APK 构建全部完成。
4. 成功后在本次运行页面的 **Artifacts** 下载 `7NKernel-1.5.0-debug-apk`。

该工作流尚未在本项目的 GitHub 仓库实际运行；只有 GitHub Actions 成功完成后，才能确认 Kotlin 编译和 APK 产物有效。

## 可选签名 Release 构建

仅在你拥有自己的发布签名密钥时启用。不要把 keystore 或密码提交到 Git 仓库。

在 GitHub 仓库 Settings → Secrets and variables → Actions 中添加：
- `ANDROID_KEYSTORE_BASE64`：keystore 文件的 Base64 内容
- `ANDROID_KEYSTORE_PASSWORD`：keystore 密码
- `ANDROID_KEY_ALIAS`：密钥别名
- `ANDROID_KEY_PASSWORD`：密钥密码

然后在 Actions 手动运行工作流，并将 `build_release` 设为 `true`。工作流会使用这些 Secrets 构建并上传 Release APK。若尚未创建自己的签名密钥，请保持 `false`，先使用 Debug APK 验证。

## 当前验证状态
- 本地静态检查：可执行，需以 `tools/validate_project.py` 实际输出为准。
- JVM 单元测试：已添加 4 项，尚未在此环境执行。
- Android Lint：配置在 CI 中，尚未执行。
- APK 编译、模拟器与实体设备测试：尚未执行。

## 安全与功能边界
原 HTML 中的驱动刷入是前端定时器模拟，不是真实内核操作。本项目不会伪装成已刷入内核，也不实现游戏作弊、瞄准辅助、透视、反作弊绕过或规避录屏检测。设备诊断页仅读取 Android 公开系统信息。

## 下一步验收
- GitHub Actions 完整通过并生成 Debug APK
- 至少一台实体设备安装启动，检查页面、导航、参数保存与 JSON 导入/导出
- 根据 Lint 与实机反馈修复问题，再考虑签名配置和 Release APK
