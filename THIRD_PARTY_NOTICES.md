# 第三方来源与修改说明

- Demeter 0.9.26：Bruce Ravel 及贡献者。Perl 覆盖文件来自该项目，保留各文件中的版权及许可文字。见 `licenses/Demeter-Artistic.txt` 和上游 https://github.com/bruceravel/demeter 。本发行中的外观、排版、导入及配置改动属于本地兼容修改，不是官方发行。
- 原始应用图标来自 Demeter，保留原图案并生成高分辨率尺寸资源，不声明原创。Atoms 图标沿用现有 Demeter 应用资源。
- gnuplot 6.0.6：Thomas Williams、Colin Kelley 及贡献者。只分发修改补丁和构建脚本，不随 DMG 分发其修改后完整源码或编译二进制。见 `licenses/gnuplot-Copyright.txt`。上游联系方式与原源码中的版权信息保留。本地构建增加 `demeter-macos-v0.3.0-pre` 版本标识。如另行分发修改后的 gnuplot 二进制，须另行遵守上游的补丁、版本和维护者联系信息等要求。
- wxWidgets 3.0.5.1：本项目提供 Cocoa 回调生命周期修复脚本，在用户机器上构建。wxWidgets 的原许可见其下载源码；https://www.wxwidgets.org/about/licence/ 。
- MacPorts 及 ports 配方：来源 https://www.macports.org/ 与 https://github.com/macports/macports-ports 。配方补丁保留为差异文件，完整依赖于安装时从上游下载。
- Ifeffit、GCC、Perl、PDL、X.Org 及其他传递依赖：安装器从固定配方指定的来源下载，其许可分别适用。DMG 不打包当前用户的完整运行时，也不授予第三方软件的额外许可。

本包没有用户配置、最近打开文件记录、实验数据或个人访问凭据。公开发布前应检查仓库最终内容，并由仓库所有者提供实际的维护与反馈渠道。不可将本说明理解为替所有组件授予一种统一许可证。

本项目维护与反馈地址：https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/issues 。现有根目录 MIT 许可的适用范围见 NOTICE.md。
