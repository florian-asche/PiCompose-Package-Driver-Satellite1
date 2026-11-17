# Abweichungen von der Stage `02-stage-audiodriver-satellite1-v1.0`

Dieses Dokument beschreibt, was das Paket `picompose-driver-satellite1` anders macht als die gleichnamige Stage im [PiCompose](https://github.com/florian-asche/PiCompose)-Repository. Verglichen wurde mit dem Stand von `main` (Commit `784abfb`, 17.11.2026). Gepflegt wird es auf Deutsch, auf Wunsch des Entwicklers.

Pflege: Jede Änderung, die eine Abweichung neu schafft, ändert oder beseitigt, aktualisiert dieses Dokument. Prüfe dabei gegen den aktuellen `main` von PiCompose.

## Nicht im Paket

- **Die Pakete von FutureProofHomes.** Der Schritt `01-driver/03-run-chroot.sh` lädt den Custom-Kernel (`linux-image-6.18.34-fusb302-rpi-v8`), `satellite1-rpi-setup` und `satellite1-rpi-sdk` von GitHub und installiert sie mit `dpkg -i`. Das bleibt in PiCompose. Ein Paket kann beim Installieren keine anderen Pakete installieren, und ein Dienst dafür ist nicht gewollt. Ohne diese Pakete funktioniert der Satellite1 nicht.
- **Pakete der Stage** (`01-driver/01-packages`: `git`, `dkms`, `curl`, `wget`, `i2c-tools`, `libasound2-plugins`, `alsa-utils`). Das Paket hängt nur von `wireplumber` und `alsa-utils` ab, weil es nur die Konfiguration und den Lautstärke-Dienst enthält.

## Anders gelöst

- Die Unit-Datei liegt in `/usr/lib/systemd/system/` statt in `/etc/systemd/system/`. Aktiviert wird sie durch `dh_installsystemd` im Paket (beim Installieren aktiviert, aber nicht gestartet) statt durch `systemctl enable` im Chroot der Stage. Der Inhalt der Unit ist unverändert.
- `51-disable-acp.conf` liegt am selben Ort wie in der Stage, ist aber als Conffile gekennzeichnet.

## Zusätzlich im Paket

- `Provides`, `Conflicts` und `Replaces` für `picompose-audio-driver`.

## Unverändert

- `51-disable-acp.conf`, `configure_audio.sh` und `configure_audio.service` sind Byte für Byte die Dateien der Stage. Die Unit nennt `seeed-voicecard.service` in `After=` und `Wants=`, die es in diesem Paket nicht gibt, genau wie in der Stage.
