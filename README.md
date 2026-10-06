# BeK Otto Lost — Team-Website

Statische Team-Website für **BeK Otto Lost** (Valorant): Verfügbarkeit, Team Comps und Strats — gehostet auf **GitHub Pages** mit **Firebase** (Realtime Database + Storage) für teamweite Synchronisation.

| | |
|---|---|
| **Live** | https://spassigxd.github.io/zero-synergy-availability/ |
| **Repository** | https://github.com/spassigxD/zero-synergy-availability |

---

## Projekt

- **BeK Otto Lost** — interne Valorant-Team-Website (Availability, Comps, Strats)
- **Technik:** reines HTML/CSS/JavaScript, keine Build-Pipeline
- **Hosting:** GitHub Pages (`master`-Branch); Push auf `master` aktualisiert die Live-Seite nach ca. 1–2 Minuten
- **Backend:** Firebase-Projekt **`znrgy-ccb87`** (Realtime Database + Storage, Blaze-Plan für Storage)

---

## Live-URLs

| Seite | URL |
|-------|-----|
| **Availability** (Start) | https://spassigxd.github.io/zero-synergy-availability/ |
| **Team Comps** | https://spassigxd.github.io/zero-synergy-availability/team-comps.html |
| **Strats** (Map-Übersicht) | https://spassigxd.github.io/zero-synergy-availability/strats.html |
| **Strats pro Map** (Beispiel Ascent) | https://spassigxd.github.io/zero-synergy-availability/strats-map.html?map=ascent |

Weitere Map-Slugs: `bind`, `breeze`, `fracture`, `haven`, `lotus`, `pearl`, `corrode`, `split`.

---

## Seiten & Features

Gemeinsame **Navigation** auf allen Seiten: **Availability** | **Team Comps** | **Strats**

### Availability (`index.html`, `app.js`)

- Wochenplan **Montag–Sonntag**, Zeiten **13:00–23:00** (stündlich)
- Spieler als Spalten; Zellen per Klick einfärben (Grün / Gelb / Rot / Löschen)
- **Firebase Realtime Database:** Pfad `teams/zero-synergy/grid` — teamweiter Live-Sync
- Ohne gültige `firebase-config.js`: nur **localStorage** in diesem Browser (Banner „Firebase nicht konfiguriert“)

### Team Comps (`team-comps.html`, `team-comps.js`)

- Raster: **5 Spieler** × **9 Maps** (max. **3 Agenten** pro Zelle)
- **Bearbeiten-Modus:** Agenten aus Sidebar per **Drag & Drop** (Desktop) oder **Zell-Picker** (Mobile)
- Map-Namen verlinken auf die jeweilige Strats-Map-Seite
- **Firebase:** `teams/zero-synergy/comps`
- Fallback ohne Firebase: localStorage

### Strats (`strats.html`, `strats.js` → `strats-map.html`, `strats-map.js`)

- **Map-Liste** (9 Maps) → Detailseite pro Map
- Upload **PDF, JPG, PNG, WebP** (mehrere Dateien = ein Strat); Firebase **Storage** + Metadaten in RTDB
- **Attack / Defence:** Tabs und Upload-Umschalter; Strats als **Gruppen** mit Galerie, Titel, Beschreibung, Reihenfolge
- Link **Valoplant** (`https://valoplant.gg/`) für Lineups / neue Strats
- Hinweise bei `file://`, fehlender Config oder Storage-Problemen (siehe `LIVE-SETUP.md`)

### Noch nicht umgesetzt

- **Challengermode** / **Discord**-Integration — nicht implementiert
- **Gankster**-Scrims — nur manuell (keine API-Anbindung)

---

## Team & Spielerlisten

Roster (Quelle im Code, Availability und Team Comps gleich):

| Kontext | Datei | Spieler (`PLAYERS`) |
|---------|--------|---------------------|
| **Availability** | `app.js` | `Fynn`, `Muchel`, `Bjarne`, `Lucas`, `Jona` |
| **Team Comps** | `team-comps.js` | `Fynn`, `Muchel`, `Bjarne`, `Lucas`, `Jona` |

Kernteam: **Fynn**, **Muchel**, **Bjarne**, **Lucas**, **Jona**.

---

## Firebase (`znrgy-ccb87`)

| Dienst | Pfad / Bucket |
|--------|----------------|
| **Realtime Database** | `teams/zero-synergy/grid` — Availability |
| | `teams/zero-synergy/comps` — Team Comps |
| | `teams/zero-synergy/strats-meta/{mapSlug}` — Strats-Metadaten (Gruppen, URLs, …) |
| **Storage** | `teams/zero-synergy/strats/{mapSlug}/` — hochgeladene Dateien |

**Konfiguration**

- `firebase-config.js` enthält `apiKey`, `authDomain`, `databaseURL`, `projectId` und für Strats **`storageBucket`** (z. B. `znrgy-ccb87.firebasestorage.app`)
- Die Datei ist **nicht** in `.gitignore` — für GitHub Pages muss sie im Repo liegen (Web-API-Keys sind öffentlich vorgesehen)
- Vorlagen: `firebase-config.example.js`, `firebase-config.TEMPLATE.js`
- **Storage-Regeln:** `storage.rules` im Repo → in der Console veröffentlichen
- **Blaze-Plan** nötig für Firebase Storage

**Weitere Dokumentation im Repo**

| Datei | Inhalt |
|-------|--------|
| [SETUP-FIREBASE.md](./SETUP-FIREBASE.md) | Projekt anlegen, RTDB-Regeln, Storage, Config |
| [LIVE-SETUP.md](./LIVE-SETUP.md) | Meldungen auf der Live-Seite, Deploy der Config |
| [TROUBLESHOOTING-UPLOAD.md](./TROUBLESHOOTING-UPLOAD.md) | Upload bleibt bei 0 %, Bucket, Console-Checks |

Console (Direktlinks): [Realtime Database](https://console.firebase.google.com/project/znrgy-ccb87/database) · [Storage](https://console.firebase.google.com/project/znrgy-ccb87/storage)

---

## Lokal entwickeln

Firebase und Storage funktionieren **nicht** bei `file://` (Doppelklick auf HTML). Lokalen HTTP-Server verwenden:

```bash
py -m http.server 8765
```

Dann im Browser öffnen, z. B.:

- http://127.0.0.1:8765/
- http://127.0.0.1:8765/team-comps.html
- http://127.0.0.1:8765/strats-map.html?map=ascent

`firebase-config.js` mit echten Werten aus der Firebase Console befüllen (siehe `SETUP-FIREBASE.md`).

---

## Wichtige Dateien

| Datei | Rolle |
|-------|--------|
| `index.html` | Availability-Seite |
| `app.js` | Availability-Logik, Grid, Firebase-Sync |
| `team-comps.html` / `team-comps.js` | Team-Comps-UI & Sync |
| `strats.html` / `strats.js` | Strats-Map-Übersicht |
| `strats-map.html` / `strats-map.js` | Strats pro Map (Upload, Gruppen, Attack/Defence) |
| `styles.css` | Gemeinsames Layout & Komponenten |
| `firebase-config.js` | Firebase-Web-Config (deploy mit committen) |
| `storage.rules` | Firebase Storage Security Rules |

---

## Bekannte Hinweise

### Cache-Busting (GitHub Pages)

Statische Assets nutzen Query-Parameter `?v=…`, damit Browser nach Deploy nicht alte JS/CSS laden. Bei Änderungen Version in den HTML-Dateien erhöhen, z. B.:

| Seite | Stand (Beispiel) |
|-------|------------------|
| `team-comps.html` | `styles.css?v=39`, `team-comps.js?v=39` |
| `index.html` | `?v=28` |
| `strats-map.html` | `styles.css?v=36`, `strats-map.js?v=35` |

### Git & Deploy

```bash
git push origin master
```

→ GitHub Pages baut die Site aus `master` (kein separater Build-Schritt).

---

## Lizenz / Kontakt

Internes Team-Tool — Nutzung und Keys nur im Team-Kontext. Bei Übergabe: zuerst `SETUP-FIREBASE.md` und `firebase-config.js` prüfen, dann Live-URLs testen.
