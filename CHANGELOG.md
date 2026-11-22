# Changelog

## v0.3.0-pre — 2026-11-23

- Public release package for macOS 27.2.x / arm64 with online source-build DMG and complete compatibility source archive.
- Add Athena's WXT foreground menu bridge, mirroring menu state and routing selections to the main application. GUI acceptance pending.
- Include existing Light/Dark appearance integration, readability and periodic-table layout patches, native application launchers and Cocoa gradient fix.
- Include the data-import temporary filename collision fix and failed/canceled import guards.
- Include the WXT gray zoom-box and right-click coordinate patches. GUI acceptance pending.
- Add English README, Chinese guide, troubleshooting, contribution and security reporting documents, license boundaries and SHA-256 manifests.
- Keep fixed MacPorts snapshot, private runtime location and refusal to overwrite unrelated installations.

### Validation boundaries

The DMG was assembled locally and its image checksum verified. Installer shell/Python syntax, payload integrity and prerequisites were checked. Plot patches were dry-run against saved source, and the private gnuplot was rebuilt with a PNG smoke test. Menu dispatch was checked with a simulated menu tree, including disabled/unknown commands and checkbox state. These are not a clean installation or interactive GUI acceptance test.

## Earlier local packages

Local v1/v2 packages preceded this public release. Their compatibility payloads did not contain all later import, heading, mouse and menu fixes. They should not be described as equivalent to v0.3.0-pre.
