# Demeter for macOS 27.2.x（Apple Silicon）

用于安装 **Athena、Artemis、Hephaestus 和 Stand-alone Atoms** 的社区兼容安装器。

本项目将一台 macOS **27.2.1 / arm64** 电脑上的安装经验整理为可检查、可重建的联网安装 DMG。发行版本：**v0.3.0-pre，2026-11-23**。本项目不是 Demeter 官方发行版。

> **安装方式：打开 DMG → 打开 Demeter Installer → 开始联网安装。**
> 安装器在 Terminal 中下载和编译软件。它不是离线运行时，也不是拖入 Applications 即可使用的完整二进制套件。Xcode 首次设置、macOS 安全确认仍需要用户完成。首次编译可能持续数小时。

## 1. 适用范围

- 支持范围：macOS **27.2.x**，Apple Silicon（`arm64`）。当前依据为 macOS 27.2.1 本机安装。
- Intel Mac、其他 macOS 大版本不在此版本的自动安装范围内。
- 必须安装并完成首次启动设置的**完整 Xcode**，仅有 Command Line Tools 不够。
- 至少预留 **30 GiB** 空间。实际编译可能需要更多，请留出余量。
- 安装期间需要稳定网络，访问 Apple 开发工具、MacPorts、GitHub 和 CPAN 等源站。
- 当前版本要求用户主目录路径只含 ASCII 字母、数字及 `_ . / -`，不含空格。数据文件路径另行处理，可以与安装路径限制不同。
- 无需安装 Chrome 或任何浏览器扩展。

## 2. DMG 安装哪些软件

1. **私有 MacPorts 2.12.7**：安装到 `~/demeter-mac/prefix`，保留原有系统 MacPorts/Homebrew。
2. **固定版本的 ports 配方**：锁定提交 `98f7ca3d00a5633a3231b253e8eac2272d2e033d`，应用本项目的配方补丁。
3. **GCC 15 / Fortran**：为 Ifeffit 等旧科学计算组件提供构建工具。本机记录为 GCC 15.3.0。
4. **Ifeffit 1.2.13 和 Perl 绑定**：供 Demeter 调用计算引擎。
5. **Perl 5.34、Wx Perl、wxWidgets 3.0** 及 Demeter 依赖；绘图依赖还包括 wxWidgets 3.2。
6. **Demeter 0.9.26**：提供 Athena、Artemis、Hephaestus、Stand-alone Atoms。
7. **PDL::Filter 2.097**：补装独立拆分出的 Perl 数值处理模块。
8. **gnuplot 6.0.6、WXT/Cairo/X11 支持和 xorg-server**：供绘图使用。默认使用 WXT，不依赖浏览器。X11 不注册全局自动启动服务。
9. **四个 Finder 应用入口**：创建在 `~/Applications`（当前用户的“应用程序”文件夹）。运行文件保留在 `~/demeter-mac`，不能删除或移动该目录。

其他依赖由锁定的 MacPorts 配方解析。最终清单保存到 `~/demeter-mac/installed-ports.txt`。本项目未包含商业 FEFF 许可证，也不承诺安装任何需单独授权的 FEFF 版本。

## 3. 对 macOS 做了哪些兼容修复

### 构建与启动

- GCC / libtool：设置 `lt_cv_sys_max_cmd_len=262144`，对应本机安装过程中遇到的构建问题。
- Ifeffit：避免 Demeter 重复安装并占用 Perl Ifeffit 模块文件。
- CFITSIO：给旧 Alien 构建配方明确 CFITSIO 最低版本参数。
- X11：移除私有安装不需要的全局 LaunchAgent / LaunchDaemon 写入和启停操作。
- wxWidgets 3.0：修正 Cocoa 渐变绘制回调所引用数据的生命周期，将修复库放入独立目录。Artemis、Hephaestus、Atoms 启动时加载该库。
- 原生应用启动器：Finder 图标通过编译的启动程序调用正确的 Perl、库路径和配置目录，解决直接双击无法正常启动的问题。

### 外观与排版

- 四个应用提供英文 **Appearance → Light / Dark**，新安装默认 **Light**，各应用分别保存选择。
- Athena：浅色模式使用深色分区标题和标签，修复 `Current group` 等白字难读问题。冻结和警告状态保留有意义的颜色。
- Athena / Artemis：调整彩色绘图按钮样式和间距，减少多余灰色背板。
- 周期表：调整元素按钮尺寸、间距和边框，修复 tooltip 格式参数问题，并更新 Db 元素标记。
- 图标：保留原始图案，生成高分辨率尺寸资源。原始图案的细节仍受来源分辨率限制。

### 数据导入

- Athena：对含非 ASCII 字符的输入路径使用独立临时目录，避免原始文件临时副本与格式转换插件的输出文件同名而互相覆盖。
- 取消或失败的导入增加对象检查，避免对不存在的数据对象调用 `source` 导致退出。
- 成功导入后恢复原始来源路径。此修复针对已定位的导入故障，不代表所有数据格式均经过验证。
- OneDrive 文件仍应先完成本地下载，云端占位文件不能等同于可读取的完整数据。

### Athena 绘图交互（需要 GUI 验收）

- Athena 使用单独编译的 gnuplot，其他应用继续使用常规绘图程序。
- WXT 中右键拖动：半透明灰色矩形加深色边框，提示放大范围。
- 右键单击：显示该点横纵坐标。
- 同位置再次单击（约 6 像素范围内）：删除该点标记；换位置单击：保留已有标记并新增。
- 新图、缩放或重绘会清除标记。坐标不是自动保存的数据注释。
- 该补丁已完成本机编译和非交互绘图检查，**尚未完成完整鼠标交互验收**，不应称为所有 Mac 上都已验证。

### 绘图窗口置顶时的 Athena 菜单（需要 GUI 验收）

- 镜像 Athena 的 File、Group、Energy、Mark 等菜单到独立 WXT 绘图进程。
- 菜单命令传回 Athena 执行，同步禁用状态，避免先点击主窗口。
- 已做编译、模拟菜单命令测试和非交互绘图检查；实际前台菜单和对话框操作仍待验收。

## 4. 安装步骤

1. 安装完整 Xcode，首次打开并完成许可和组件设置。
2. 下载同一次 Release 的 `Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg` 与 `SHA256SUMS.txt`，核对校验值。
3. 打开 DMG，先阅读随附说明，再打开 **Demeter Installer.app**。
4. 如果 macOS 因开发者身份未验证而阻止打开，先核对来源，在“系统设置 → 隐私与安全性”中按系统提示处理。此版本使用本地临时签名，**没有 Developer ID 公证**。无需关闭系统安全保护。
5. 点击“开始联网安装”，Terminal 会显示下载、编译、补丁和应用生成过程。保持终端运行，避免电脑休眠或断网。
6. 等待出现“安装步骤完成”，然后在 Finder 的 `~/Applications` 中依次打开四个应用。
7. 完成下面的验收。安装完成信息说明脚本执行成功，不等于每一种科学分析和图形操作都已验证。

### 可复制的 Terminal 命令

检查系统和架构：

```bash
sw_vers -productVersion
uname -m
```

检查 Xcode：

```bash
xcode-select -p
xcodebuild -version
```

如 Xcode 已装在标准位置但尚未选中：

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

在下载文件所在文件夹核对 DMG（将文件与校验清单放在一起）：

```bash
shasum -a 256 Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg
```

将输出与同次 Release 的 `SHA256SUMS.txt` 中对应一行对照。SHA-256 用于发现文件改变，不替代对发布者的信任判断。

安装运行后，在另一终端查看日志：

```bash
tail -f "$HOME/demeter-mac/logs/install.log"
```

打开应用所在目录：

```bash
open "$HOME/Applications"
```

源码方式运行（在本仓库目录中）：

```bash
unzip compatibility-source-v0.3.0-pre.zip -d demeter-source
cd demeter-source
python3 source/build-payload.py
bash source/install.sh source/resources --check-only
bash source/install.sh source/resources
```

`--check-only` 只检查系统、架构、Xcode、空间和路径，不执行安装。安装脚本本身无需 `sudo`。不要用 `sudo` 运行整套安装。

## 5. 安装后的验收

- **Athena**：导入已下载的已知标准数据，检查 E / k / R 图，保存并重新打开项目；比较处理结果与已知参考。
- **Artemis**：打开已有测试项目，检查绘图、路径与拟合流程。任何依赖外部 FEFF 程序的步骤应单独验证其路径与授权。
- **Hephaestus**：点击元素，检查吸收边、荧光线、元素数据与周期表排版。
- **Stand-alone Atoms**：打开已知结构文件，检查解析及输出结果。
- **外观**：切换 Light / Dark，再重启检查选择是否保留；检查标题、按钮和周期表是否可读。
- **Athena 鼠标补丁**：右键四个方向拖动并观察框选边界；单击同点两次，再单击不同位置；检查缩放后清除标记的行为。

本机原有四个应用此前已由使用者确认可以使用。**新版安装器尚未在干净的另一台 Mac 或干净用户环境中执行完整端到端安装**，建议作为预发布版本分享，并请测试者记录环境及失败日志。

## 6. 中断、冲突和日志

- 安装日志：`~/demeter-mac/logs/install.log`。
- 应用日志：`~/demeter-mac/logs/athena.log`、`artemis.log`、`hephaestus.log`、`atoms.log`。
- 正常退出后的安装可再次运行，已安装依赖可复用；失败阶段可能重新下载或重新构建。
- 安装器会拒绝覆盖不属于本安装器的 `~/demeter-mac` 或同名应用。
- 强制终止可能留下 `~/demeter-mac/.install-lock`。确认已无安装进程后才可删除该锁目录再重试。
- 已有窗口不会自动切换到新版本。保存项目并完全退出后重新打开。
- 反馈时提供系统版本、芯片类型、失败阶段和脱敏后的日志。不要上传实验原始数据或私人目录内容。

## 7. 移除

先备份项目和偏好，再退出所有相关程序。移除本安装器创建的 `~/Applications` 下四个应用及 `~/demeter-mac` 即可移除私有安装。请先检查目录内容，不要删除其他软件或用户数据。安装器启动副本位于 `~/Library/Application Support/DemeterInstaller`。

## 8. 仓库和 Release 文件

- `source/install.sh`：联网安装的主流程。
- `source/compatibility/`：Perl 覆盖模块、配方补丁、绘图补丁、启动器及构建脚本。
- `source/Installer.swift`：安装器窗口源码。
- `source/build-payload.py`：生成内部校验包。
- `source/build-dmg.sh`：本地重建 DMG。
- `THIRD_PARTY_NOTICES.md` 与 `licenses/`：第三方来源和许可说明。
- `RELEASE_NOTES.md`：GitHub Release 可直接使用的说明。
- `Demeter-macOS-27.2-视频讲解.pptx`：可编辑演示文稿，每页附中文讲稿备注。

大文件 DMG / PPTX 建议上传到 GitHub **Releases**，源码和 README 放在仓库。将本版本标记为 **Pre-release**。发布地址：https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/releases 。

仓库将完整源码放在 compatibility-source-v0.3.0-pre.zip 中，解压后得到本节所述 source/ 和 licenses/。

## 9. 上游资料

- [Demeter 官方文档](https://bruceravel.github.io/demeter/)
- [Demeter 源码](https://github.com/bruceravel/demeter)
- [固定 MacPorts ports 快照](https://github.com/macports/macports-ports/tree/98f7ca3d00a5633a3231b253e8eac2272d2e033d)
- [gnuplot 官方网站](http://www.gnuplot.info/)
- [Apple：打开来源不明开发者的 App](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unknown-developer-mh40616/mac)

修复问题请先反馈给本项目的发布者。除非确认可以在未修改的官方版本重现，不要将社区补丁故障直接归因于上游。
