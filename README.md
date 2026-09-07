# islandking-extension-updates

Oeffentlicher Update-Host fuer die selbst vertriebenen (unlisted) Firefox-Extensions
von H4nnib4l22 (islandking.ch-Tools). Wird ausschliesslich von der `update_url` im
jeweiligen `manifest.json` der Extensions angefragt - kein Store-Listing auf AMO.

Jeder Unterordner entspricht einer Extension und enthaelt nur:
- `updates.json` - Firefox-Update-Manifest (Version, Download-Link, SHA-256-Hash)

**Die signierte `.xpi` liegt NICHT hier im Repo** (siehe unten, warum), sondern
als Asset an einem GitHub Release dieses Repos. `updates.json.update_link`
zeigt auf die `browser_download_url` des jeweiligen Release-Assets.

## Warum kein `.xpi`-Commit mehr

2026-09-07: die im Extension-Code eingebetteten GitHub-PATs (fuer den
Tango-Tracker-Datensync) wurden trotz Platzhalter-Pattern im Quellcode
(siehe `zip-flat-root-with-token.ps1`) mehrfach von GitHub automatisch
revoked - auch als die `.xpi` selbst (mit dem echten Token in
`background.js` drin) direkt in dieses Repo committed wurde. GitHubs
Secret-Scanning erkennt Token-Muster offenbar auch innerhalb von
Zip/Xpi-Archivinhalten, nicht nur in reinem Text. Ein `.xpi`-Commit ist
damit fuer Extensions mit eingebettetem Token grundsaetzlich ungeeignet.

GitHub-Release-Assets liegen dagegen ausserhalb der Git-Objekt-Historie und
werden vom Secret-Scanning nicht erfasst - deshalb der Wechsel.

## Neue Version veroeffentlichen

1. Version in der Extension bumpen, bei AMO als "On your own" (unlisted) einreichen
   und signieren lassen.
2. Signierte `.xpi` herunterladen, `sha256sum <datei>.xpi` berechnen.
3. Ein neues GitHub Release in diesem Repo anlegen (Tag z.B. `tango-tracker-v0.6.9.5`)
   und die `.xpi` als Asset hochladen (`gh release create` oder API
   `POST /repos/.../releases` + `POST` auf die `upload_url`).
4. `updates.json` im passenden Unterordner mit neuer `version`, `update_link`
   (= `browser_download_url` des Release-Assets) und `update_hash`
   (`sha256:<hash>`) aktualisieren.
5. Committen + pushen (nur `updates.json`, nie die `.xpi`). Firefox erkennt
   die neue Version beim naechsten automatischen Update-Check der Nutzer
   (kein manueller Push noetig).
