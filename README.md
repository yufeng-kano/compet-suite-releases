# Compet Consulting Suite Releases

This public repository hosts Windows x64 and macOS releases for the private `yufeng-kano/compet-suite` source repository.

## Windows

Latest installer:

<https://github.com/yufeng-kano/compet-suite-releases/releases/latest/download/CompetConsultingSuiteSetup.exe>

Releases are currently **not code-signed**. Windows SmartScreen will warn on first run; choose "More info" → "Run anyway". Installed copies update themselves. Authenticode signing is planned once a company certificate is available.

## macOS

Quit the app first, then paste this into Terminal. It picks the Apple Silicon or Intel build, installs it to Applications, and opens it. Run the same command again to update; macOS copies do not update themselves.

```sh
ARCH=$([ "$(sysctl -n hw.optional.arm64 2>/dev/null)" = 1 ] && echo arm64 || echo x64) && curl -fL -o /tmp/CompetConsultingSuite.zip "https://github.com/yufeng-kano/compet-suite-releases/releases/latest/download/CompetConsultingSuite-macOS-$ARCH.zip" && rm -rf "/Applications/競爭力企業管理顧問.app" && ditto -x -k /tmp/CompetConsultingSuite.zip /Applications && xattr -cr "/Applications/競爭力企業管理顧問.app" && open "/Applications/競爭力企業管理顧問.app"
```

Manual install: download `CompetConsultingSuite-macOS-arm64.zip` (Apple Silicon) or `CompetConsultingSuite-macOS-x64.zip` (Intel) from the latest release, unzip, drag the app to Applications, then run:

```sh
xattr -cr "/Applications/競爭力企業管理顧問.app"
```

The app is **not signed or notarized** by Apple. Without the `xattr` step macOS reports it as damaged and refuses to open it.

## Scope

- Windows x64 and macOS (Apple Silicon arm64, Intel x64). macOS builds are available from v2.0.0.
- Each GitHub Release contains `CompetConsultingSuiteSetup.exe`, `*-full.nupkg`, and `RELEASES` for Squirrel.Windows auto-update via `update.electronjs.org`, plus `CompetConsultingSuite-macOS-arm64.zip` and `CompetConsultingSuite-macOS-x64.zip`.
- This repository does not contain the private application source, company data, signing certificates, or CI/CD credentials.
- Publicly downloadable installers do not grant access to company data. Application access remains protected by Google OIDC and the server-side account allowlist.

## Release policy

Releases are created by GitHub Actions in the private source repository (a new `desktop/package.json` version on `main` creates a `desktop-v*` tag and publishes).

Published releases are immutable. Existing tags and assets must not be overwritten; fixes require a new SemVer version.

Do not manually upload, replace, or delete update assets. Support and source changes belong in the private project repository.
