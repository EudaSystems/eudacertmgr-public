<!-- This README is overwritten on each release by the release workflow in EudaSystems/eudacertmgr. Do not edit directly. -->
# EudaCertMgr&trade; — Releases

This repository hosts the downloadable installer tarballs for [**EudaCertMgr**](https://eudasystems.com/), the automated TLS/SSL certificate manager for Linux and Windows fleets.

The source code lives in a private repository. This repo exists so customers and prospects can download release artifacts directly.

## Download

Grab the installer for your architecture from the [latest release](https://github.com/EudaSystems/eudacertmgr-public/releases/latest):

- `eudacertmgr-installer-linux-amd64.tar.gz`
- `eudacertmgr-installer-linux-arm64.tar.gz`
- `SHA256SUMS`

Verify and install:

```bash
sha256sum -c SHA256SUMS
tar -xzf eudacertmgr-installer-linux-amd64.tar.gz
sudo ./eudacertmgr-installer
```

The installer is interactive. Full documentation and feature list will appear here once the first release is published.

## License

EudaCertMgr is commercial software. The installer prompts for license acceptance before performing any system changes. See [eudasystems.com](https://eudasystems.com/) for licensing.

Copyright &copy; 2026 Euda Systems, Inc. All Rights Reserved.
