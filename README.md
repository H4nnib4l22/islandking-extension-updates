# islandking-extension-updates

Oeffentlicher Update-Host fuer die selbst vertriebenen (unlisted) Firefox-Extensions
von H4nnib4l22 (islandking.ch-Tools). Wird ausschliesslich von der `update_url` im
jeweiligen `manifest.json` der Extensions angefragt - kein Store-Listing auf AMO.

Jeder Unterordner entspricht einer Extension und enthaelt:
- `updates.json` - Firefox-Update-Manifest (Version, Download-Link, SHA-256-Hash)
- die signierte `.xpi` der jeweils aktuellen Version

## Neue Version veroeffentlichen

1. Version in der Extension bumpen, bei AMO als "On your own" (unlisted) einreichen
   und signieren lassen.
2. Signierte `.xpi` herunterladen, `sha256sum <datei>.xpi` berechnen.
3. `.xpi` in den passenden Unterordner legen, `updates.json` dort mit neuer
   `version`, `update_link` und `update_hash` (`sha256:<hash>`) aktualisieren.
4. Committen + pushen. Firefox erkennt die neue Version beim naechsten
   automatischen Update-Check der Nutzer (kein manueller Push noetig).
