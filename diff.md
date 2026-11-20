# Abweichungen von der Stage `02-stage-audiodriver-satellite1-v1.0`

Dieses Dokument beschreibt, was dieses Repository anders macht als die gleichnamige Stage im [PiCompose](https://github.com/florian-asche/PiCompose)-Repository. Verglichen wurde mit dem Stand von `main` (Commit `fe56bb4`, 20.11.2026). Gepflegt wird es auf Deutsch, auf Wunsch des Entwicklers.

Pflege: Jede Änderung, die eine Abweichung neu schafft, ändert oder beseitigt, aktualisiert dieses Dokument. Prüfe dabei gegen den aktuellen `main` von PiCompose.

## Anders gelöst

- **Die Pakete kommen aus dem APT-Repository.** `01-driver/03-run-chroot.sh` lädt `satellite1-rpi-setup-trixie` 1.5-1 und `satellite1-rpi-sdk` 0.1.6 von GitHub und installiert sie mit `dpkg -i`. Hier lädt der Paket-Build dieselben Dateien (gleiche URLs), prüft den SHA256 und veröffentlicht sie unverändert, nach `trixie-nightly` (Push auf `main`) und `trixie` (Release).
- **Ein Prüfsummen-Check, den die Stage nicht hat.** Ändert FutureProofHomes ein Release-Asset, schlägt der Build fehl.

## Nicht im Paket

- **Pakete der Stage** (`01-driver/01-packages`: `git`, `dkms`, `curl`, `wget`, `i2c-tools`, `libasound2-plugins`, `alsa-utils`, `python3-venv`, `flashrom`). Die Pakete von FutureProofHomes bringen ihre Abhängigkeiten selbst mit (`python3-venv`, `python3-pip`, `flashrom`, `libasound2-plugins`). Wer `git`, `dkms`, `curl`, `wget`, `i2c-tools` und `alsa-utils` braucht, installiert sie weiter in der Stage.
- **Das Marker-Paket `picompose-driver-satellite1`** mit `Provides`, `Conflicts` und `Replaces` für `picompose-audio-driver` gibt es nicht mehr. Die Pakete von FutureProofHomes liefern das nicht, also verhindert APT keine Installation neben einem anderen `picompose-driver-*`-Paket, siehe `todo.md`.
- **`configure_audio` und `51-disable-acp.conf`** gibt es nicht mehr (siehe die früheren Versionen dieses Repositories). Die Lautstärke kommt aus `picompose-core`, das Mikrofon braucht `satellite1-rpi-setup` 1.5-1 oder neuer.

## Zusätzlich

- `debian/skip-lintian`: lintian läuft für dieses Repository nicht, weil die Pakete von FutureProofHomes lintian-Fehler haben.
