# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Decide whether this package is needed:** The three packages of FutureProofHomes (kernel, `satellite1-rpi-setup`, `satellite1-rpi-sdk`) are installed by the PiCompose image, not by this package, because a package cannot install packages and a service that does it is not wanted. The package only carries the WirePlumber rule and the volume service. If that is not worth a package, drop the repository.
2. **Use an APT repository of FutureProofHomes:** As soon as FutureProofHomes provides one, add `Depends` on `satellite1-rpi-setup` and `satellite1-rpi-sdk` (and the kernel if it is still needed), and the image no longer downloads them.
3. **Packages of the stage that are not dependencies yet:** The stage also installs `git`, `dkms`, `curl`, `wget`, `i2c-tools`, `libasound2-plugins` and `alsa-utils`. Only `alsa-utils` (and `wireplumber`) are dependencies of this package. Check which of the others the Satellite1 setup needs.
4. **Test on hardware:** Nothing was tested on a Satellite1.
