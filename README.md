# XAFS Demeter Perl for macOS

Community installer for **Athena, Artemis, Hephaestus and Stand-alone Atoms** on **macOS 27.2.x / Apple Silicon (arm64)**.

[中文安装指南](INSTALL.zh-CN.md) · [Downloads](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/releases/tag/v0.3.0-pre) · [Troubleshooting](TROUBLESHOOTING.md) · [License scope](NOTICE.md)

> **Pre-release v0.3.0-pre · 2026-11-23.** Based on an existing macOS 27.2.1 installation. A clean end-to-end install on a second Mac has not yet been verified. Recent plot mouse/menu patches still require interactive GUI acceptance. The DMG is an **online source-build installer**: full Xcode, internet access and several hours for the first build may be required. It uses an ad hoc signature and is not notarized by Apple.

## Download

Open the [v0.3.0-pre release](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/releases/tag/v0.3.0-pre) and download:

- [`Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg`](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/releases/download/v0.3.0-pre/Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg)
- `SHA256SUMS.txt` from the same release.
- Optional: `compatibility-source-v0.3.0-pre.zip` for the complete installer sources, overlays, patches and third-party notices.

The DMG contains the installer, compatibility sources, license notices and instructions. Runtime dependencies download and compile on the destination Mac. It does not copy the original user's runtime, settings or experimental data.

## Requirements

- macOS **27.2.x**, Apple Silicon **arm64**. The system/architecture checks intentionally reject other versions and Intel Macs.
- **Full Xcode**, opened once to complete its license and initial components. Command Line Tools alone are insufficient for this installer.
- Internet access to MacPorts, GitHub, CPAN and source archives used by the pinned recipes.
- At least **30 GiB** free space; more may be needed for retained build trees.
- Home directory containing only ASCII letters, digits and `_ . / -`, with no spaces. This restriction applies to the installation path, not all input data filenames.
- Chrome and browser extensions are not required.

## Install

1. Download the DMG and checksum list from the release above.
2. In Terminal, run `shasum -a 256` on the DMG and compare the value with `SHA256SUMS.txt`.
3. Open the DMG, read its guide, and launch **Demeter Installer.app**.
4. If macOS blocks the unnotarized app, verify the source first, then follow Apple's per-app **Open Anyway** process in System Settings → Privacy & Security. Do not disable system-wide protections.
5. Click **开始联网安装** (Start Online Installation). Keep Terminal running while downloads, builds and patches complete.
6. After the installer reports completion, open the four apps in **`~/Applications`** and perform the acceptance checks below.

```bash
# System and Xcode checks
sw_vers -productVersion
uname -m
xcode-select -p
xcodebuild -version
```

If full Xcode is already installed in its standard location but not selected:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

The installer itself runs as the current user, without `sudo`.

```bash
# Run from the directory containing the downloaded release files.
shasum -a 256 Demeter-macOS-27.2-online-arm64-v0.3.0-pre.dmg

# Follow installation progress in another Terminal window.
tail -f "$HOME/demeter-mac/logs/install.log"

# Open the current user's application directory.
open "$HOME/Applications"
```

## Installed components

1. **Private MacPorts 2.12.7**, with ports pinned to commit `98f7ca3d00a5633a3231b253e8eac2272d2e033d`.
2. **GCC 15 / GFortran**, used to build the older scientific backend. The existing installation records GCC 15.3.0.
3. **Ifeffit 1.2.13** and its Perl binding.
4. **Perl 5.34, Wx Perl, wxWidgets 3.0**, and numerical dependencies. Plotting also uses **wxWidgets 3.2**.
5. **Demeter 0.9.26**, providing Athena, Artemis, Hephaestus and Atoms.
6. **PDL::Filter 2.097**, installed separately if absent.
7. **gnuplot 6.0.6**, WXT/Cairo/X11 support and **xorg-server** dependencies. WXT is the default interactive terminal.
8. **Four compiled Finder launchers**, each with its own configuration and log directory.

Dependencies resolve from the pinned recipes. The final list is written to `~/demeter-mac/installed-ports.txt`. External FEFF executables and any required commercial licenses must be verified separately.

## Included compatibility work

- **Build recipes:** GCC/libtool command-length configuration; Ifeffit Perl file-ownership conflict; CFITSIO version parameters; private-prefix X11 startup adjustments.
- **Cocoa crash:** rebuild the wxWidgets 3.0 gradient callback with corrected data lifetime, kept in an isolated library directory.
- **Finder launch:** native wrappers set the matching Perl, module search path, dynamic libraries and working directory.
- **Appearance:** English **Appearance → Light / Dark** in all four tools, default Light for a fresh install and separate saved preferences.
- **Readability:** dark text for Athena section headings in Light mode, retained freeze/warning colors, adjusted Athena/Artemis plotting buttons, periodic-table spacing, borders and tooltip formatting.
- **Original icons:** preserve the original artwork and add higher-resolution icon sizes. Detail remains limited by the original source artwork.
- **Data import:** isolate temporary copies of non-ASCII paths from plugin output filenames, guard canceled/failed imports, and restore the original input source path after successful conversion. OneDrive placeholder files still need to be downloaded locally.
- **Athena plot mouse patch:** gray translucent zoom rectangle with dark edges; right-click coordinate notes; click the same location again to remove a note; click elsewhere to add another. Notes clear on a new plot, zoom or redraw.
- **Athena plot menu bridge:** mirror Athena menus into the separate WXT plotting process and route selections back to Athena. Disabled entries remain disabled. This addresses the need to click the main window before accessing File / Group / Energy / Mark when a plot is frontmost.

**Recent interaction patches:** compilation, non-interactive plotting and menu protocol checks passed locally. Full foreground-menu, dialog, keyboard and mouse acceptance remains pending. Do not present these checks as completed GUI validation.

## Locations and update behavior

| Item | Location |
| --- | --- |
| Runtime, sources, build caches | `~/demeter-mac` |
| Private MacPorts | `~/demeter-mac/prefix` |
| Application launchers | `~/Applications` |
| Separate application preferences | `~/demeter-mac/state/{athena,artemis,hephaestus,atoms}` |
| Installer log | `~/demeter-mac/logs/install.log` |
| Application logs | `~/demeter-mac/logs/{athena,artemis,hephaestus,atoms}.log` |

Do not move or delete `~/demeter-mac` while using the installed apps. The installer refuses unrelated existing directories or same-name applications. Normal retries reuse installed dependencies; a forcibly terminated run may leave a lock. Save projects and fully quit existing windows before using an updated runtime. Original installations in a different directory require a separate migration; this release does not silently replace them.

## Acceptance checks

- **Athena:** import a known standard, plot E / k / R, compare processing with known results, save and reopen a project.
- **Artemis:** open a known project, inspect paths and run the intended fit workflow. Validate external FEFF configuration separately.
- **Hephaestus:** select multiple elements and check edges, lines, element data and layout.
- **Atoms:** open a known structure and check parsing and output.
- **All tools:** switch Light/Dark, restart and confirm readable controls and persisted preferences.
- **Mouse patch:** drag in all four directions; click the same and different points; check marker cleanup after redraw.
- **Menu bridge:** keep E / R plots frontmost, open File / Group / Energy / Mark, execute a harmless command, test disabled entries and dialog commands, then repeat with multiple plot windows.

The existing four applications were previously confirmed usable by the original user. That does not establish clean-machine install success or validate every scientific workflow.

## Sources and rebuilding

This repository distributes the full source tree as **`compatibility-source-v0.3.0-pre.zip`**, also available in Releases. Extract it to obtain `source/`, `licenses/` and the license notices. It contains readable Perl overlays, patch files, the Swift installer, native launcher generator and all build scripts. It contains no prebuilt scientific runtime.

```bash
unzip compatibility-source-v0.3.0-pre.zip -d demeter-source
cd demeter-source
python3 source/build-payload.py
bash source/install.sh source/resources --check-only
# Start an actual installation only when ready:
bash source/install.sh source/resources
```

`--check-only` performs prerequisite checks without installing. To rebuild the DMG from the same extracted tree:

```bash
bash source/build-dmg.sh
```

Build output is local and ignored by Git. The script requires the same full Xcode and Apple Silicon environment. The default DMG build includes the guide, source and licenses; presentation files are optional local extras.

## Documentation and contribution

- [Chinese installation guide](INSTALL.zh-CN.md)
- [Troubleshooting and safe removal](TROUBLESHOOTING.md)
- [Changes and test boundaries](CHANGELOG.md)
- [Contributing and issue reports](CONTRIBUTING.md)
- [Security reporting](SECURITY.md)
- [License scope](NOTICE.md) and [third-party notices](THIRD_PARTY_NOTICES.md)

Questions and reports: [repository Issues](https://github.com/gaoxiao0041/XAFS_DemeterPerl_MacOS/issues). Remove private paths, credentials and experimental data from logs before sharing.

## License

Repository-owned installer/integration code and documentation use the existing [MIT license](LICENSE), where the author owns those rights. **Upstream-derived overlays, patches, icons and downloaded dependencies retain their own licenses.** MIT does not relicense Demeter, gnuplot or the other dependencies. See [NOTICE.md](NOTICE.md), [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md), and `licenses/` in the source ZIP and DMG. No commercial FEFF license is included.

## Upstream

[Demeter documentation](https://bruceravel.github.io/demeter/) · [Demeter source](https://github.com/bruceravel/demeter) · [MacPorts](https://www.macports.org/) · [pinned ports snapshot](https://github.com/macports/macports-ports/tree/98f7ca3d00a5633a3231b253e8eac2272d2e033d) · [gnuplot](http://www.gnuplot.info/) · [Apple app-opening guidance](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unknown-developer-mh40616/mac)
