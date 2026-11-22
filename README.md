# Demeter for macOS

Online installer for **Athena, Artemis, Hephaestus and Stand-alone Atoms** on **macOS 27.2.x / Apple Silicon (arm64)**.

[Download v0.3.0-pre](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/releases/tag/v0.3.0-pre) · [Installation](INSTALL.md) · [Troubleshooting](TROUBLESHOOTING.md)

> **Pre-release.** Based on an existing macOS 27.2.1 installation. Clean-machine installation and recent plot mouse/menu interactions still need validation. The installer is ad hoc signed and not Apple-notarized.

## Requirements

- macOS **27.2.x**, Apple Silicon **arm64**.
- Full **Xcode**, with its license and initial setup completed.
- Internet access and at least **30 GiB** free space; builds may need more.
- An ASCII home-directory path without spaces, using letters, digits or `_ . / -`.

## Installation

1. Download `Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg` and `SHA256SUMS.txt` from [Releases](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/releases/tag/v0.3.0-pre).
2. Verify the DMG's SHA-256 value against the matching checksum list.
3. Open the DMG and **Demeter Installer.app**, then click **Start Online Installation**.
4. Keep Terminal open until completion. The first build may take hours.
5. Open the four apps in **`~/Applications`** and test them with known data.

If macOS blocks the app, verify its source and follow Apple's per-app **Open Anyway** process in System Settings → Privacy & Security. Do not disable global protections.

```bash
shasum -a 256 Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg
tail -f "$HOME/demeter-mac/logs/install.log"
open "$HOME/Applications"
```

## What is installed

- Private **MacPorts 2.12.7**, with a [pinned ports snapshot](https://github.com/macports/macports-ports/tree/98f7ca3d00a5633a3231b253e8eac2272d2e033d).
- **GCC 15 / GFortran**, **Ifeffit 1.2.13**, **Perl 5.34**, Wx and PDL dependencies.
- **Demeter 0.9.26**, **wxWidgets 3.0/3.2**, **gnuplot 6.0.6**, WXT/Cairo and X11 dependencies.
- Four native Finder launchers with separate preferences and logs.

Runtime files stay in **`~/demeter-mac`**. Do not move or delete this directory while using the apps. External FEFF programs and any required licenses must be configured separately.

## Compatibility patches

- Build fixes for GCC/libtool, Ifeffit module conflicts, CFITSIO and private-prefix X11 installation.
- Cocoa gradient callback lifetime fix and native application launchers.
- English **Appearance → Light / Dark**, default Light, readable headings and improved button/periodic-table layouts.
- Athena import fixes for non-ASCII paths and failed/canceled imports. OneDrive files must be available locally.
- Athena WXT gray zoom rectangle, right-click coordinate markers and a foreground plot-menu bridge. **Interactive acceptance is pending.**

## Source and rebuilding

`compatibility-source-v0.3.0-pre.zip`, available here and in Releases, contains the complete installer sources, readable overlays, patches, rebuild scripts and third-party licenses.

```bash
unzip compatibility-source-v0.3.0-pre.zip -d demeter-source
cd demeter-source
python3 source/build-payload.py
bash source/install.sh source/resources --check-only
# To install:
bash source/install.sh source/resources
# To rebuild the DMG instead:
bash source/build-dmg.sh
```

## License and support

[MIT](LICENSE) applies to repository-owned code and documentation. Upstream-derived files, icons and dependencies retain their own licenses; see [NOTICE](NOTICE.md) and [third-party notices](THIRD_PARTY_NOTICES.md).

Report reproducible problems with redacted logs in [Issues](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/issues). See [CHANGELOG](CHANGELOG.md), [CONTRIBUTING](CONTRIBUTING.md) and [SECURITY](SECURITY.md).

Unofficial community distribution. [Demeter documentation](https://bruceravel.github.io/demeter/) · [Demeter source](https://github.com/bruceravel/demeter) · [Apple app-opening guidance](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unknown-developer-mh40616/mac)
