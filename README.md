# gz-_ASSETS

Public download host for the Vyzorix container engine's Debian rootfs tarballs.

## Why this repo exists

The app lives in a **private** repository. GitHub serves a private repo's release
assets only to authenticated callers, so a phone in a member of the public's hands
cannot fetch them — a 404. These assets must be reachable by any device, so they live
here, in a public repo.

Nothing secret is here. Each tarball is a Debian 12 (bookworm) userspace assembled
from Debian's own archive; the packages are public upstream. The private code — the
engine, the connector, the app — stays in the app repo.

## What is published

Per ABI (`arm64`, `armeabi-v7a`, `x86_64`):

| File | Purpose |
|:--|:--|
| `debian-core-<abi>.tar.gz` | The container's base userspace. Bundled in the APK for `arm64`; downloaded by other ABIs on first boot. |
| `debian-extended-<abi>.tar.gz` | Optional tooling (`git`, `python3`, `tmux`), downloaded on demand. |
| `BUILD-<abi>.md` | The build record: package sets and resolved versions. |
| `SHA256SUMS` | Every tarball. **The device verifies against this before extracting.** |

## The download URL

The app fetches from `releases/latest/download/`:

```
https://github.com/VinnsEdesigner/gz-_ASSETS/releases/latest/download/debian-core-<abi>.tar.gz
```

A release marked **latest** is what devices pick up. Override the base with
`-ProotfsBaseUrl=...` to point a fork elsewhere.

## Verifying a download by hand

```bash
curl -LO https://github.com/VinnsEdesigner/gz-_ASSETS/releases/latest/download/SHA256SUMS
curl -LO https://github.com/VinnsEdesigner/gz-_ASSETS/releases/latest/download/debian-core-arm64.tar.gz
sha256sum -c SHA256SUMS --ignore-missing
```

## How these are built

In the app repository: `scripts/build-rootfs.sh --arch <abi>`. The resulting tarballs
are uploaded here as a release. Do not build here — the build script and the rootfs
specification (`doc/ANDROID_SERVICE_ARCHITECTURE.md` §6.1.1) live with the app.
