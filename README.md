# m365print-releases

Release channel for **M365 Cloud Print** by Acid Networks: the Windows client
(agent and print app) and the packages for the print box. The updaters built
into the client and the box read this repository. Devices update themselves,
so there is normally no reason to download anything here by hand.

## Contents

| File | Read by |
|---|---|
| `manifest.json`, `manifest.json.sig` | Windows client (`agent.exe update`) |
| `box.json`, `box.json.sig` | Print box (`m365-box-updater`) |
| Releases `vX.Y.Z` | Binaries the manifests point to |

Each manifest names the builds, the channels (`stable`, `pilot`, `v1`) and per
tenant pins. Tenants appear only as a hash of their tenant ID, never by name.

## Release files

From 2.1.0 on, files are named `<product>_<version>_<arch>`:

| File | Installs as |
|---|---|
| `m365-agent_X.Y.Z_amd64.exe`, `m365-agent_X.Y.Z_arm64.exe` | `agent.exe` (Windows client) |
| `m365-cloudprinter_X.Y.Z_amd64.msix`, `m365-cloudprinter_X.Y.Z_arm64.msix` | `CloudPrinter.msix` (print app, Windows 11 24H2 and later) |
| `m365-fetcher_X.Y.Z_amd64.deb` | print box service |
| `m365-box-updater_X.Y.Z_amd64.deb` | print box updater |

Releases up to 2.0.9 use the older names `agent.exe`, `CloudPrinter.msix`,
`agent-arm64.exe` and `CloudPrinter-arm64.msix`. Each release also carries
`SHA256SUMS`.

## Verification

Updaters accept a manifest only with a valid SSH signature (`ssh-keygen -Y
verify`, namespace `m365print-update` for `manifest.json` and
`m365print-box-update` for `box.json`) from one of two hardware keys, an
expiry date in the future and a serial no lower than the highest they have
already seen. They then check every download against the size and SHA-256 in
the manifest. Client and box trust the same two keys.

| Key | Fingerprint |
|---|---|
| A (in use) | `SHA256:SsZZG1tXeJ8B9Fj0tV5GdI/47lHEs1s0m0yl693YGX0` |
| B (backup) | `SHA256:1m0YgPvAOgmbR+2W/PND3LcLAu9HrtBf2BR/Ysw7UIU` |

Windows binaries and packages are also Authenticode-signed by
`CN=Acid Networks, O=Acid Networks, C=DE`. That certificate is self-issued and
becomes trusted on a device only when the client installs it, so on a fresh
machine Windows reports the signature as untrusted. The SHA-256 in the signed
manifest is what proves a download.

## Changes

Only the release tooling in the main project writes here (`scripts/release.sh`,
`scripts/manifest.sh`); every manifest change is signed with a hardware key.
Please do not open pull requests.
