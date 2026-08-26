# Pix Fotostudio – Multi-Galerie-System

Kein Terminal, kein Python mehr nötig. Alles läuft über den Browser.

## Dateien

| Datei | Wohin auf GitHub |
|---|---|
| `index.html` | `galerie/index.html` (ersetzt deine bisherige `galerie.html`) |
| `admin.html` | `galerie/admin.html` |
| `manifest.json` | `galerie/manifest.json` |

Die Ordner `galerie/galleries/<slug>/img/...` und `.../sentinel.bin` werden
automatisch von `admin.html` angelegt – die musst du nicht manuell erstellen.

**Deinen alten `galerie/sentinel.bin` und `galerie/img/` Ordner kannst du
danach löschen**, die werden vom neuen System nicht mehr benutzt.

## Einmalige Einrichtung

### 1. Admin-Passwort setzen
Das Standard-Passwort für `admin.html` ist **`changeme`** – bitte unbedingt
ändern, bevor du live gehst:

1. Öffne `admin.html` in einem beliebigen Browser (auch lokal von der Festplatte, per Doppelklick)
2. Öffne die Browser-Konsole (F12 → „Console")
3. Tippe ein: `await sha256hex("dein-neues-passwort")` und drücke Enter
4. Kopiere den ausgegebenen Hash
5. Öffne `admin.html` in einem Texteditor, suche `ADMIN_PW_HASH` und ersetze den Wert durch deinen neuen Hash
6. Speichern, hochladen

### 2. GitHub Personal Access Token erstellen
Damit die Admin-Webapp Fotos direkt zu GitHub hochladen kann:

1. Auf GitHub: Profilbild oben rechts → **Settings**
2. Ganz unten links: **Developer settings**
3. **Personal access tokens → Fine-grained tokens → Generate new token**
4. Name z. B. „Pix Galerie Admin"
5. **Repository access**: „Only select repositories" → dein `pix`-Repo auswählen
6. **Permissions → Repository permissions → Contents**: auf **„Read and write"** stellen
7. Token erzeugen, **sofort kopieren** (wird nur einmal angezeigt!)
8. In `admin.html` oben unter „GitHub-Zugang" einfügen → „Speichern"

Der Token bleibt nur in deinem Browser (localStorage) und wird nirgendwo
sonst gespeichert. Wenn du ihn verlierst/widerrufen willst: einfach ein
neues Token erstellen und das alte auf GitHub löschen.

### 3. Hochladen
`index.html`, `admin.html` und `manifest.json` wie in der Tabelle oben ins
Repo hochladen (einmalig, ganz normal über GitHub, wie bisher auch).

## Laufender Betrieb – neue Galerie anlegen

1. `admin.html` im Browser öffnen (Lesezeichen setzen – Link ist z. B.
   `https://<username>.github.io/<repo>/galerie/admin.html`, funktioniert
   aber auch einfach lokal von der Festplatte)
2. Admin-Passwort eingeben
3. **Galerie-Kennung** vergeben, z. B. `mueller-hochzeit-2026`
   (wird automatisch in kleine Buchstaben + Bindestriche umgewandelt)
4. **Kundenpasswort** eingeben oder auf „Generieren" klicken
5. Fotos per Drag & Drop reinziehen oder anklicken zum Auswählen
6. „Galerie speichern & hochladen" klicken
7. Fertig – die Galerie ist sofort unter deiner normalen `index.html`-URL
   mit dem gewählten Passwort erreichbar (kein separater Link pro Kunde
   nötig, alle Kunden landen auf derselben Login-Seite)

## Bestehende Galerie bearbeiten

Unten in `admin.html` unter „Bestehende Galerien" auf **„Bearbeiten"**
klicken. Neue Fotos hochladen **ersetzt alle alten Fotos** dieser Galerie
automatisch (kein manuelles Löschen des alten Ordners mehr nötig). Das
Passwort kannst du dabei gleich mitändern.

## Galerie löschen

Unten auf **„Löschen"** klicken – entfernt alle Fotos, das `sentinel.bin`
und den Eintrag aus der Galerie-Liste.

## Wie der zentrale Login funktioniert

Der Kunde gibt auf `index.html` einfach sein Passwort ein – ohne Galerie-
Namen oder speziellen Link. Die Seite probiert das Passwort im Hintergrund
gegen jede vorhandene Galerie, bis eine passt, und zeigt dann genau deren
Fotos.

## Sicherheitshinweise

- Das Admin-Passwort schützt nur die Bedienoberfläche vor Neugierigen –
  die eigentliche Sicherheit kommt vom GitHub-Token (nur du hast ihn) und
  von der AES-256-Verschlüsselung der Fotos selbst.
- Gib dem Token nur Zugriff auf genau dieses eine Repository.
- Nutze `admin.html` nur auf deinem eigenen, gesperrten Rechner – nicht auf
  öffentlichen/geteilten Geräten, da der Token im Browser gespeichert wird.
