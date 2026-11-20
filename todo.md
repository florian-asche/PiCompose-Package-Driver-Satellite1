# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Use an APT repository of FutureProofHomes:** As soon as FutureProofHomes provides an APT repository, drop the download in `debian/rules`.

2. **Test on hardware:** Nothing was tested on a Satellite1. The packages are published unchanged, the build was only tried as a cross build for arm64 in a container.

3. **Stop the download in the PiCompose stage:** `01-driver/03-run-chroot.sh` of the stage downloads the same packages. Change it to `apt-get install satellite1-rpi-setup-trixie satellite1-rpi-sdk` after the first release of this repository (the PiCompose repository is read only for this guide, the developer changes it).

4. **Remove `picompose-driver-satellite1` from the APT repository:** The workflow that removes a package from the APT repository has to be run for `picompose-driver-satellite1` in both suites, as the package is no longer built.

5. **Marker for one audio driver:** The packages of FutureProofHomes cannot provide `picompose-audio-driver`, so APT does not stop an installation next to another `picompose-driver-*` package. Decide whether that is acceptable.
