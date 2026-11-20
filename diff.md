# Abweichungen von der Stage `02-stage-audiodriver-satellite1-v1.0`

Dieses Dokument beschreibt, was das Paket `picompose-driver-satellite1` anders macht als die gleichnamige Stage im [PiCompose](https://github.com/florian-asche/PiCompose)-Repository. Verglichen wurde mit dem Stand von `main` (Commit `784abfb`, 17.11.2026). Gepflegt wird es auf Deutsch, auf Wunsch des Entwicklers.

Pflege: Jede Änderung, die eine Abweichung neu schafft, ändert oder beseitigt, aktualisiert dieses Dokument. Prüfe dabei gegen den aktuellen `main` von PiCompose.

## Nicht im Paket

- **Die Pakete von FutureProofHomes.** Der Schritt `01-driver/03-run-chroot.sh` lädt den Custom-Kernel (`linux-image-6.18.34-fusb302-rpi-v8`), `satellite1-rpi-setup` und `satellite1-rpi-sdk` von GitHub und installiert sie mit `dpkg -i`. Das bleibt in PiCompose. Ein Paket kann beim Installieren keine anderen Pakete installieren, und ein Dienst dafür ist nicht gewollt. Ohne diese Pakete funktioniert der Satellite1 nicht.
- **Pakete der Stage** (`01-driver/01-packages`: `git`, `dkms`, `curl`, `wget`, `i2c-tools`, `libasound2-plugins`, `alsa-utils`). Das Paket hängt von nichts ab, weil es keine Dateien enthält.
- **`configure_audio.sh` und `configure_audio.service`.** Sie haben beim Start die Lautstärke der Standard-Senke per `wpctl` auf 100 % gesetzt und den ALSA-Zustand gespeichert (`alsactl store`). Das Skript setzte keine Mixer-Controls, die Helfer dafür waren ungenutzt. Den Startwert von 100 % sollte jetzt `audio.volume = 1.0` für `alsa_output.*` in `50-alsa-output.conf` von `picompose-core` liefern. Auf einem Satellite1 mit `satellite1-rpi-setup` 1.5-1 stand die Senke dabei auf 1.00. Mit einem frischen WirePlumber-Zustand ist es nicht geprüft, siehe `todo.md`. Eine Karte mit bereits gespeichertem Zustand behält diesen Wert, das Paket setzt ihn nicht mehr zurück.
- **`51-disable-acp.conf`.** Die Stage installierte die Datei, das Paket enthält sie nicht mehr. Das Mikrofon funktioniert ab `satellite1-rpi-setup` 1.5-1 ohne sie (FutureProofHomes/Satellite1-RPi-Setup#12 und #14), mit einer älteren Version fehlt es. Das Paket kann die Version nicht per `Depends` erzwingen, weil die Pakete von FutureProofHomes nicht im APT-Repository liegen, das Image muss mindestens 1.5-1 installieren. Die Namen der PipeWire-Nodes ändern sich zu `alsa_output.platform-soc_sound.output` und `alsa_input.platform-soc_sound.input`.

## Zusätzlich im Paket

- `Provides`, `Conflicts` und `Replaces` für `picompose-audio-driver`.
