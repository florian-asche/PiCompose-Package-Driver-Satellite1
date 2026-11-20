# PiCompose-Package-Driver-Satellite1

[![Version](https://github.com/florian-asche/PiCompose-Package-Driver-Satellite1/actions/workflows/version.yml/badge.svg?branch=main)](https://github.com/florian-asche/PiCompose-Package-Driver-Satellite1/actions/workflows/version.yml) [![GitHub Release Version](https://img.shields.io/github/v/release/florian-asche/PiCompose-Package-Driver-Satellite1?label=version)](https://github.com/florian-asche/PiCompose-Package-Driver-Satellite1/releases) [![GitHub License](https://img.shields.io/github/license/florian-asche/PiCompose-Package-Driver-Satellite1)](https://github.com/florian-asche/PiCompose-Package-Driver-Satellite1/blob/main/LICENSE)

This repository publishes the packages of [FutureProofHomes](https://github.com/FutureProofHomes) for the Satellite1 to the [PiCompose APT repository](https://github.com/florian-asche/PiCompose-Repository), unchanged. It replaces the downloads of the stage `02-stage-audiodriver-satellite1-v1.0` of the [PiCompose](https://github.com/florian-asche/PiCompose) image, so the image can install them with `apt`.

## Contents

| Package | Version | Source |
| --- | --- | --- |
| `satellite1-rpi-setup-trixie` | 1.5-1 | [Satellite1-RPi-Setup](https://github.com/FutureProofHomes/Satellite1-RPi-Setup/releases/tag/v1.5-1-trixie) |
| `satellite1-rpi-sdk` | 0.1.6 | [Satellite1-RPi-SDK](https://github.com/FutureProofHomes/Satellite1-RPi-SDK/releases/tag/v0.1.6) |

Both are `arm64` only. `debian/rules` downloads the `.deb` files while the package is built, checks the SHA256 and copies them to the build output without changing them, so the packages in the APT repository are the packages of FutureProofHomes, with their version. Both are licensed under GPL-3.0, the source is in the repositories above. The files of this repository itself (`debian/`) are MIT licensed.

The package `satellite1-rpi-setup-trixie` replaces `satellite1-rpi-setup` (the version for other releases). There is no marker package, so nothing stops an installation next to another `picompose-driver-*` package.

## Update a package

1. In `debian/rules`, change the version, the download URL and the checksum (`sha256sum` of the downloaded file). If a package gets a new name, also change `debian/control` and `sources.yml` of the APT repository.
2. Open a pull request with a version label. The version of the source package (the first entry of `debian/changelog`) only counts the releases of this repository, the packages keep the version of FutureProofHomes.

`debian/skip-lintian` skips lintian in the build, because the packages of FutureProofHomes have problems (no copyright file and others) that cannot be fixed without changing them.

## Versioning and changelog

The version of the package is the first entry of `debian/changelog`, which is part of the package. You do not edit it by hand, the workflows write it.

1. **Pull request:** Every pull request needs exactly one of the labels `major`, `minor` and `patch`. Tick one box under "Version bump" in the description or set the label by hand. The check "Version label" fails until there is exactly one.
2. **Merge to `main`:** The workflow "Version" raises the version, adds the pull request titles to `debian/changelog`, commits it to `main`, creates the tag `v<version>` as pre-release and starts the build, which publishes to the suite `trixie-nightly`.
3. **Release:** Turn the pre-release into a release in the GitHub UI. That publishes the same version to the suite `trixie`.
4. **Any other branch:** `<version>+dev<timestamp>.<sha>`, kept as workflow artifact for 14 days, nothing is published.

The repository needs the secret `HUB_DISPATCH_TOKEN` and a branch protection of `main` that requires the check "Version label" and lets `github-actions[bot]` push, see the [DEVELOPMENT.md](https://github.com/florian-asche/PiCompose-Repository/blob/main/DEVELOPMENT.md) of the APT repository. The repository also has to be listed in `sources.yml` of the APT repository.

## Local build

```sh
sudo apt install build-essential debhelper devscripts fakeroot lintian
dpkg-buildpackage -us -uc -b
lintian ../picompose-driver-satellite1_*.deb
```

## License

This project is released under the [MIT License](LICENSE).
