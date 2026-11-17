# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Use an APT repository of FutureProofHomes:** The packages of FutureProofHomes (kernel, `satellite1-rpi-setup`, `satellite1-rpi-sdk`) are installed by the PiCompose image. As soon as FutureProofHomes provides an APT repository, add `Depends` on them to this package and let the image stop downloading them.

2. **Drop the custom kernel:** The image installs the custom kernel `linux-image-6.18.34-fusb302-rpi-v8`. Remove it there when the Satellite1 no longer needs it.

3. **Test on hardware:** Nothing was tested on a Satellite1.
