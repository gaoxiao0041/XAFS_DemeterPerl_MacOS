# Contributing

Use Issues for reproducible installation and compatibility reports. Include system version, chip, Xcode version, release/tag, failing step, minimal reproduction and redacted logs. Do not post credentials or private experimental data.

Before proposing a patch, extract the compatibility source archive. Keep upstream-derived changes minimal and retain copyright notices and patch context. Describe which original source/version the change targets. Avoid replacing the pinned ports snapshot without retesting affected recipes.

For installer changes, run shell/Python syntax checks, `--check-only`, checksum checks and patch dry-runs. For GUI changes, record actual interactive results with E/R windows, menu states, dialogs, Light/Dark modes and multiple windows. A build or simulated event test alone is insufficient. For scientific processing changes, compare known standard data with expected output.

Please describe validation honestly and retain the pre-release boundary until a clean-machine install and required interactive checks pass. Report issues with this modified distribution here first, rather than assuming the upstream release behaves identically.
