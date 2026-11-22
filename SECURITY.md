# Security policy

This is an experimental community installer. No supported security-response timeframe is promised. Use only the release links in this repository and verify SHA-256 values from the matching release. Checksums detect changed files but do not authenticate the publisher by themselves.

Do not disable Gatekeeper, SIP or other global protections to run this package. The current app has an ad hoc signature and no Apple notarization. Review its source and use Apple's per-app opening process only if you trust the publisher.

Do not include tokens, personal paths, unpublished data or sensitive logs in public reports. If GitHub offers a private vulnerability-reporting option for this repository, use it. Otherwise open a minimal issue requesting a private contact method without disclosing exploit details or sensitive material. No private reporting channel is assumed to be configured.

The installer downloads upstream software and builds it locally. Third-party vulnerabilities remain subject to the corresponding upstream maintenance policies. The release is pinned for reproducibility and does not automatically track every upstream security update.
