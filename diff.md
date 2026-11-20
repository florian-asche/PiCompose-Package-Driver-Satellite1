# Abweichungen von der Stage `02-stage-audiodriver-satellite1-v1.0`

Dieses Dokument beschreibt, was das Paket `picompose-driver-satellite1` anders macht als die gleichnamige Stage im [PiCompose](https://github.com/florian-asche/PiCompose)-Repository. Verglichen wurde mit dem Stand von `main` (Commit `784abfb`, 17.11.2026). Gepflegt wird es auf Deutsch, auf Wunsch des Entwicklers.

Pflege: Jede Änderung, die eine Abweichung neu schafft, ändert oder beseitigt, aktualisiert dieses Dokument. Prüfe dabei gegen den aktuellen `main` von PiCompose.

## Nicht im Paket

- **Die Pakete von FutureProofHomes.** Der Schritt `01-driver/03-run-chroot.sh` lädt den Custom-Kernel (`linux-image-6.18.34-fusb302-rpi-v8`), `satellite1-rpi-setup` und `satellite1-rpi-sdk` von GitHub und installiert sie mit `dpkg -i`. Das bleibt in PiCompose. Ein Paket kann beim Installieren keine anderen Pakete installieren, und ein Dienst dafür ist nicht gewollt. Ohne diese Pakete funktioniert der Satellite1 nicht.
- **Pakete der Stage** (`01-driver/01-packages`: `git`, `dkms`, `curl`, `wget`, `i2c-tools`, `libasound2-plugins`, `alsa-utils`). Das Paket hängt nur von `wireplumber` ab, weil es nur die WirePlumber-Konfiguration enthält.
- **`configure_audio.sh` und `configure_audio.service`.** Sie haben beim Start die Lautstärke der Standard-Senke per `wpctl` auf 100 % gesetzt und den ALSA-Zustand gespeichert (`alsactl store`). Das Skript setzte keine Mixer-Controls, die Helfer dafür waren ungenutzt. Den Startwert von 100 % sollte jetzt `audio.volume = 1.0` für `alsa_output.*` in `50-alsa-output.conf` von `picompose-core` liefern. `device.routes.default-sink-volume` aus `51-default-volume.conf` greift hier vermutlich nicht, weil `51-disable-acp.conf` die Routes der Karte abschaltet. Beides ist nicht auf Hardware geprüft, siehe `todo.md`. Eine Karte mit bereits gespeichertem Zustand behält diesen Wert, das Paket setzt ihn nicht mehr zurück.

## Anders gelöst

- `51-disable-acp.conf` liegt am selben Ort wie in der Stage, ist aber als Conffile gekennzeichnet.

## Zusätzlich im Paket

- `Provides`, `Conflicts` und `Replaces` für `picompose-audio-driver`.

## Unverändert

- `51-disable-acp.conf` ist Byte für Byte die Datei der Stage.
