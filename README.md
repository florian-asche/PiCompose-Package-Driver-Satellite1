# PiCompose-Package-Driver-Satellite1

Package `picompose-driver-satellite1` for the [PiCompose APT repository](https://github.com/florian-asche/PiCompose-Repository). It is the audio setup for the Satellite1 of [FutureProofHomes](https://github.com/FutureProofHomes). It is the stage `02-stage-audiodriver-satellite1-v1.0` of the [PiCompose](https://github.com/florian-asche/PiCompose) image as a package.

## Contents

| Part | Files |
| --- | --- |
| WirePlumber | `/etc/wireplumber/wireplumber.conf.d/51-disable-acp.conf` turns off the ALSA card profile handling (ACP) for the card `alsa_card.platform-soc_sound` |
| Volume setup | `/usr/bin/configure_audio.sh`, `configure_audio.service` (enabled, not started at install) |
| FutureProofHomes packages | `/usr/bin/satellite1-install-fph-packages`, `satellite1-fph-packages.service` (enabled, not started at install) |

The files are the ones of the image without changes.

## The driver comes from FutureProofHomes

The image downloads three packages from the GitHub releases of FutureProofHomes while it is built and installs them with `dpkg`: the custom kernel `linux-image-6.18.34-fusb302-rpi-v8`, `satellite1-rpi-setup` and `satellite1-rpi-sdk`. This package does the same, with the same versions and in the same order, until FutureProofHomes provides an APT repository:

- `/usr/bin/satellite1-install-fph-packages` downloads the three files with `wget` and installs them with `dpkg -i`. The versions are variables at the top of the script.
- `satellite1-fph-packages.service` runs it **once, at the next boot**, after the network is online, and retries every 30 seconds if it fails. A marker file `/var/lib/picompose/satellite1-fph-packages-installed` records that it is done.
- It cannot run in the `postinst`, because `dpkg` is locked while the package is installed and cannot install other packages. You can also run `sudo satellite1-install-fph-packages` by hand.
- The kernel is only used after one more reboot.

This is a temporary exception: otherwise no package downloads anything while it is installed. Replace it with `Depends` on the packages of FutureProofHomes once they have an APT repository, see `todo.md`.

## One driver per system

All `picompose-driver-*` packages provide `picompose-audio-driver` and conflict with it. Installing one removes another, because a system has one audio device and all of them install `configure_audio.sh` and `configure_audio.service`.

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
