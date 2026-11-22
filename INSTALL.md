# Installation

## Prerequisites

macOS 27.2.x, arm64, full Xcode, internet access, at least 30 GiB free space and an ASCII home path without spaces.

```bash
sw_vers -productVersion
uname -m
xcode-select -p
xcodebuild -version
```

Open Xcode once to complete its license and components. If full Xcode is installed at the standard path but not selected:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

## DMG

1. Download the DMG and checksum list from the same Release.
2. Compare `shasum -a 256 <downloaded.dmg>` with `SHA256SUMS.txt`.
3. Open the DMG, read its guide and run **Demeter Installer.app**.
4. Click **Start Online Installation** and wait for Terminal to report completion.
5. Open the apps in `~/Applications`. Save and quit old windows before testing an update.

Run the installer as the current user, without `sudo`. The DMG downloads and builds dependencies; the first build may take hours. It uses an ad hoc signature without Apple notarization. Use Apple's per-app opening process only after verifying the source.

## Locations

| Item | Path |
| --- | --- |
| Runtime and private MacPorts | `~/demeter-mac`, `~/demeter-mac/prefix` |
| Apps | `~/Applications` |
| Separate application preferences | `~/demeter-mac/state/{athena,artemis,hephaestus,atoms}` |
| Installer log | `~/demeter-mac/logs/install.log` |
| Application logs | `~/demeter-mac/logs/{athena,artemis,hephaestus,atoms}.log` |

Do not move the runtime directory. Unrelated existing installations are preserved and may block automatic installation.

## Acceptance

- Athena: import known data, plot E/k/R, save and reopen a project.
- Artemis: check a known project, paths and the intended fit workflow; validate external FEFF separately.
- Hephaestus: check element, edge and line data.
- Atoms: load a known structure and check output.
- All apps: test Light/Dark and persisted preferences after restarting.
- Recent Athena patches: test zoom rectangles, coordinate toggling, and File/Group/Energy/Mark with E/R plots frontmost, including disabled entries and dialogs.

Compare scientific output with known results. Clean-machine installation and recent plot mouse/menu GUI acceptance remain unverified.

## Source installation

```bash
unzip compatibility-source-v0.3.0-pre.zip -d demeter-source
cd demeter-source
python3 source/build-payload.py
bash source/install.sh source/resources --check-only
bash source/install.sh source/resources
```

`--check-only` validates prerequisites without installing. Rebuild a DMG with `bash source/build-dmg.sh`.
