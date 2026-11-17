# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Use an APT repository of FutureProofHomes:** The packages of FutureProofHomes are installed by a service that downloads them from GitHub at the next boot, like the image does. As soon as FutureProofHomes provides an APT repository, replace `satellite1-install-fph-packages` and the service with `Depends` on `satellite1-rpi-setup` and `satellite1-rpi-sdk`.

2. **Drop the custom kernel:** The script also installs the custom kernel `linux-image-6.18.34-fusb302-rpi-v8`. Remove it when the Satellite1 no longer needs it.

3. **Failing dependencies of dpkg -i:** `dpkg -i` does not install missing dependencies, as in the image. Check whether the three packages need `apt-get -f install` afterwards.

4. **Test on hardware:** Nothing was tested on a Satellite1.
