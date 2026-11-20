# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Use an APT repository of FutureProofHomes:** The packages of FutureProofHomes (kernel, `satellite1-rpi-setup`, `satellite1-rpi-sdk`) are installed by the PiCompose image. As soon as FutureProofHomes provides an APT repository, add `Depends` on them to this package and let the image stop downloading them.

2. **Drop the custom kernel:** The image installs the custom kernel `linux-image-6.18.34-fusb302-rpi-v8`. Remove it there when the Satellite1 no longer needs it.

3. **Test on hardware:** Nothing was tested on a Satellite1.

4. **Test the volume on hardware:** `configure_audio.service` was removed. The default volume of 100 % should come from `audio.volume = 1.0` for `alsa_output.*` nodes in `50-alsa-output.conf` of `picompose-core`. On a Satellite1 with the ACP profile set of `satellite1-rpi-setup` 1.5-1 the sink showed 1.00, but not with a fresh `~/.local/state/wireplumber`. Check that the sink starts at 100 % with a fresh state.

5. **The package is empty:** It contains no files after `51-disable-acp.conf` was removed, it only provides `picompose-audio-driver`. Decide whether to keep it as a marker, or to drop it, or to give it `Depends` on the packages of FutureProofHomes (see 1).

6. **Install `satellite1-rpi-setup` 1.5-1 or newer in the image:** The PiCompose stage downloads `satellite1-rpi-setup` itself. Without 1.5-1 the microphone does not work, since this package no longer turns ACP off.
