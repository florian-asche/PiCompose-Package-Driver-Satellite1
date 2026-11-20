# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Use an APT repository of FutureProofHomes:** The packages of FutureProofHomes (kernel, `satellite1-rpi-setup`, `satellite1-rpi-sdk`) are installed by the PiCompose image. As soon as FutureProofHomes provides an APT repository, add `Depends` on them to this package and let the image stop downloading them.

2. **Drop the custom kernel:** The image installs the custom kernel `linux-image-6.18.34-fusb302-rpi-v8`. Remove it there when the Satellite1 no longer needs it.

3. **Test on hardware:** Nothing was tested on a Satellite1.

4. **Test the volume on hardware:** `configure_audio.service` was removed. The default volume of 100 % should now come from `audio.volume = 1.0` for `alsa_output.*` nodes in `50-alsa-output.conf` of `picompose-core`. The setting `device.routes.default-sink-volume` of `51-default-volume.conf` applies to route volumes, and `51-disable-acp.conf` turns the routes of this card off, so it probably does not reach the Satellite1. Check on a Satellite1 with a fresh `~/.local/state/wireplumber` that the sink starts at 100 %.

5. **Is `51-disable-acp.conf` still needed:** The reason is not documented, the file came with a general audio fix for all driver stages. Check on hardware whether the card works with ACP enabled, and drop the file if it does.
