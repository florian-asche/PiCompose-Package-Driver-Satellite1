# AI working guide for PiCompose-Package-Driver-Satellite1

Publishes the packages `satellite1-rpi-setup-trixie` and `satellite1-rpi-sdk` of FutureProofHomes (FPH) unchanged to the [PiCompose APT repository](https://github.com/florian-asche/PiCompose-Repository). They replace the downloads of the stage `02-stage-audiodriver-satellite1-v1.0` of the [PiCompose](https://github.com/florian-asche/PiCompose) image. `debian/rules` downloads the `.deb` files at build time and checks the SHA256. Read `README.md` for what the package contains. When this file and the code disagree, the code wins, fix this file in the same change.

**FPH = FutureProofHomes**, the maker of the Satellite1. Their repositories:

- [FutureProofHomes](https://github.com/FutureProofHomes): the organization.
- [Satellite1-RPi-Setup](https://github.com/FutureProofHomes/Satellite1-RPi-Setup): source of `satellite1-rpi-setup-trixie` (releases: `v1.5-1-trixie`).
- [Satellite1-RPi-SDK](https://github.com/FutureProofHomes/Satellite1-RPi-SDK): source of `satellite1-rpi-sdk` (releases: `v0.1.6`).
- [RPi-Kernel-Fusb302](https://github.com/FutureProofHomes/RPi-Kernel-Fusb302): the custom kernel of the Satellite1, not published by this repository.

## Rules

- **PiCompose and linux-voice-assistant are read only.** Never change anything in the PiCompose repository or in the linux-voice-assistant repository, not locally and not on GitHub. They are references for what the image does and what the voice assistant needs.
- Nothing is posted to GitHub (pull requests, discussions, comments) and nothing is pushed without the explicit consent of the developer. A human reviews every change, see `.github/AI_POLICY.md`.
- Documentation, code comments, commit and PR text are in English, whatever language the conversation is in.
- **Open points live in `todo.md`.** Read it before you start. When the developer postpones something ("we do that later") or you find something you cannot settle in this change, add an entry there, and remove the entry in the change that settles it.
- Do not edit `debian/changelog`. The workflows write it: every pull request needs exactly one of the labels `major`, `minor` and `patch` (tick one box under "Version bump" in the pull request description), and the version follows from it when the pull request is merged.
- The packages never download anything while they are installed. Where the PiCompose stage downloaded sources while it built the image, the package build downloads the same (see `debian/rules`), and no third-party sources are stored in this repository.
- The packages of FutureProofHomes are published unchanged. Do not repack them. Every download has a checksum in `debian/rules`, update the checksum together with the URL. `debian/skip-lintian` makes the shared build skip lintian, keep it only while this repository republishes packages of others.
- Hardware behavior (audio, boot configuration, kernel modules, I2C) cannot be tested here. Say that it was not tested on hardware, never claim it was.

## Commands

| Task | Command |
| --- | --- |
| Build and lint like the CI | in a `debian:trixie` container: `apt-get build-dep ./`, `dpkg-buildpackage -us -uc -b -a arm64 -d` (lintian is skipped, see `debian/skip-lintian`) |
| Build with the scripts of the APT repository | `PiCompose-Repository/scripts/build-package.sh <this directory> amd64` |
| Test a maintainer script | run it with `sh` in a container against a copy of the files it changes |

## Compatibility with Raspberry Pi OS (required for every change)

The package has to work on the Raspberry Pi OS that the PiCompose images are built on. Judge that for **every** change, before you finish, and write the result into the pull request description under "Compatibility with Raspberry Pi OS".

1. **Find out what the images use today.** Do not assume it. Read these files of the PiCompose repository (`/home/florian/repos/PiCompose` if it is checked out, otherwise `https://github.com/florian-asche/PiCompose`):
   - `.github/workflows/build-image-template.yml`: the Debian release (`releaseversion`, default `trixie`), the pi-gen repository and branch (`pi-gen-repository`, `pi-gen-version`), the user (`username`) and the runner architecture.
   - `.github/workflows/build-all.yml`: which images exist and which stages each of them builds (`stage-list`), so which images contain the stage of this package.
   - The stage directory `02-stage-audiodriver-satellite1-v1.0` and its `README`/`docs/` for what the image does today.
2. **Check this package against it.** What to look at:
   - The Debian release: do all packages in `Depends`, `Recommends` and `Build-Depends` exist in that release and in the Raspberry Pi archive (`archive.raspberrypi.com/debian`)? Check with `apt-cache policy` in a container of that release.
   - The kernel and firmware layout of Raspberry Pi OS for that release (kernel version, `/boot/firmware`, `config.txt`, `usercfg.txt`).
   - The architectures: the images are built for arm64, check whether armhf is still needed.
   - Defaults of the base system that the package relies on: the user `pi` with the UID 1000, systemd, PipeWire and WirePlumber, `cron`.
   Specific to this package:
   - The packages of FutureProofHomes are built for Debian trixie (`satellite1-rpi-setup-trixie`). Check that the release of the image is still trixie, and that their `Depends` (`python3-venv`, `python3-pip`, `flashrom`, `libasound2-plugins`) exist in that release and in the Raspberry Pi archive.
   - They are `arm64` only, check that no image needs armhf or amd64 with the Satellite1.
3. **Write down the verdict**: which release, kernel and image definition you looked at, what you checked and how (commands), what you could not check (hardware), and what has to change if the base moves, for example to a new Debian release. If a change makes the package fit only a newer or only an older base than the images use, say so and ask the developer before going on.
