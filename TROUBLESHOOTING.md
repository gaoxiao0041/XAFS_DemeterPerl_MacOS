# Troubleshooting

## Installation progress

```bash
tail -f "$HOME/demeter-mac/logs/install.log"
```

The first GCC/wxWidgets build may take hours. Check for new compiler output before treating a long build as stuck. Keep the machine powered and Terminal open. Downloads require the source hosts listed by the pinned recipes.

## Common failures

| Symptom | Check |
| --- | --- |
| Unsupported system/architecture | Run `sw_vers -productVersion` and `uname -m`; this release accepts only 27.2.x / arm64. |
| Full Xcode required | Open full Xcode, finish initial setup and select its Developer directory. |
| Prefix or application conflict | Preserve the existing directory/app. This installer only updates items it recognizes as its own. |
| Download or hash failure | Check connectivity and the failing host. Do not bypass the expected checksum. Retry. |
| Compile failure | Record the failing STEP and port, then the first relevant compiler error and the MacPorts main.log. |
| Missing Perl module | Launch through the generated app so Perl 5.34 and the overlay paths match. |
| Cocoa gradient crash | Check that the auxiliary application launcher loads the separate wx-fixed library directory. |
| Plot missing | Check the configured terminal is WXT and gnuplot has WXT support. DISPLAY/X11 checks apply if explicitly using X11. |
| Plot-frontmost menus missing | Save and quit both Athena and all plot windows, then reopen. Old processes do not acquire the new bridge. The recent bridge still needs GUI validation. |
| OneDrive import fails | Make the file available locally first. Share a minimal non-confidential example only if necessary. |

## Interrupted installation

Rerun after the previous installer has exited. Installed dependencies may be reused. A forcibly terminated run can leave `~/demeter-mac/.install-lock`. Remove that directory only after confirming no installation is still running. Never start two installers in the same prefix.

## Application logs

`~/demeter-mac/logs/athena.log`, `artemis.log`, `hephaestus.log`, `atoms.log`.

For issue reports provide macOS, architecture, Xcode version, app/installer version, failing step, expected behavior, actual behavior and a short redacted log excerpt. The original customized machine may use different log locations; these paths describe a fresh installation from this package.

## Removal

Save and back up projects/preferences, then quit all tools. Inspect `~/demeter-mac` before removing it, along with only this installer's four apps in `~/Applications`. Do not remove unrelated applications. Installer launch copies are under `~/Library/Application Support/DemeterInstaller`. Project files saved elsewhere are independent of the private runtime.
