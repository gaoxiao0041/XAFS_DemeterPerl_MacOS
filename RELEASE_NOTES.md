# v0.3.0-pre · macOS 27.2.x / Apple Silicon

Community online installer for **Athena, Artemis, Hephaestus and Stand-alone Atoms**.

### Downloads

- **DMG**: `Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg`
- **Checksums**: `SHA256SUMS.txt`
- **Complete compatibility sources**: `compatibility-source-v0.3.0-pre.zip`
- **Optional video presentation**: `Demeter-macOS-27.2-视频讲解.pptx` and `视频讲稿.md`. This presentation introduces the earlier v2 workflow; current v0.3.0 menu changes and validation boundaries are documented in the README/CHANGELOG.

### Included

- Private MacPorts, GCC/GFortran, Ifeffit, Perl/Wx/PDL, Demeter and gnuplot/X11 dependencies.
- Four native Finder launchers in `~/Applications`.
- Light/Dark appearance, readable Athena section headings, button/periodic-table layout fixes and Cocoa gradient lifetime fix.
- Non-ASCII data-path temporary-file collision fix and failed/canceled import guards.
- Athena private WXT zoom-box/coordinate patches and foreground menu bridge.
- Full installer sources, upstream patch files, rebuild scripts and license notices.

### Requirements and status

**macOS 27.2.x, arm64, full Xcode, internet access, at least 30 GiB free space and a simple ASCII home path without spaces.**

Open DMG → Demeter Installer → 开始联网安装 → wait for Terminal → open apps and perform acceptance checks.

First build may take hours. Ad hoc signature, no Apple notarization. Existing macOS 27.2.1 installation experience; clean-machine end-to-end installation is not yet verified. Recent plot menu/mouse patches require interactive GUI acceptance. Compilation and protocol checks do not establish GUI success.

Read the repository README and NOTICE before installing. Verify checksums against this release. This is an unofficial **pre-release**; external FEFF executables/licenses require separate verification.
