# HTML OS → IPA mit GitHub Actions (kein Mac nötig)

## Übersicht
Diese Methode verpackt deine `index.html` in eine native iOS-App (WKWebView-Wrapper), baut sie kostenlos auf einem GitHub Actions Mac-Runner und gibt dir eine `.ipa`-Datei zum Sideloaden mit AltStore Classic.

---

## Schritt 1 – GitHub Repository erstellen

1. Gehe zu **github.com** → **New repository**
2. Name: `htmlos-ipa` (oder beliebig)
3. **Private** auswählen (empfohlen)
4. **Create repository** klicken

---

## Schritt 2 – Dateien hochladen

Lade alle Dateien aus diesem Projekt **exakt** in dieser Ordnerstruktur hoch:

```
htmlos-ipa/
├── .github/
│   └── workflows/
│       └── build-ipa.yml
└── ios-app/
    ├── HTMLOS.xcodeproj/
    │   └── project.pbxproj
    └── HTMLOS/
        ├── AppDelegate.swift
        ├── SceneDelegate.swift
        ├── ViewController.swift
        ├── Info.plist
        ├── LaunchScreen.storyboard
        ├── index.html          ← DEINE index.html hier!
        ├── Assets.xcassets/
        │   ├── Contents.json
        │   ├── AccentColor.colorset/
        │   │   └── Contents.json
        │   └── AppIcon.appiconset/
        │       ├── Contents.json
        │       └── icon-1024.png   ← DEIN Icon hier! (1024x1024 PNG)
```

### Wichtig: Deine Dateien einfügen
- **`ios-app/HTMLOS/index.html`** → ersetzen mit deiner `index.html`
- **`ios-app/HTMLOS/Assets.xcassets/AppIcon.appiconset/icon-1024.png`** → dein App-Icon (1024×1024 PNG)

### Dateien auf GitHub hochladen (im Browser):
- Navigiere in den jeweiligen Ordner
- Klicke **Add file → Upload files**
- Ziehe die Datei rein → **Commit changes**

Für den `.github/workflows/` Ordner: Erstelle ihn manuell über  
**Add file → Create new file** → tippe `build-ipa.yml` als Pfad ein  
(GitHub erstellt Ordner automatisch wenn du `/` im Namen tippst)

---

## Schritt 3 – Build starten

1. Gehe zu deinem Repository → **Actions** Tab
2. Links siehst du **"Build IPA"**
3. Klicke **Run workflow** → **Run workflow** (grüner Button)
4. Warte ca. **10–15 Minuten** (GitHub baut auf einem echten Mac)

---

## Schritt 4 – IPA herunterladen

1. Gehe zu **Actions** → klicke auf den abgeschlossenen Workflow-Run
2. Unten bei **Artifacts** → **HTMLOS-IPA** klicken
3. ZIP wird heruntergeladen → entpacken → `HTMLOS.ipa` ist drin

---

## Schritt 5 – Mit AltStore Classic auf iPhone laden

### Vorbereitung (einmalig):
1. **AltServer** auf einem Windows-PC installieren (altstore.io)
2. **AltStore Classic** auf dem iPhone installieren (über AltServer)
3. iPhone und PC im selben WLAN

### IPA installieren:
1. `HTMLOS.ipa` auf dein iPhone übertragen (per AirDrop, iCloud Drive, oder Email)
2. In AltStore Classic → **My Apps** → **+** (oben links)
3. `HTMLOS.ipa` auswählen
4. Fertig! App erscheint auf dem Homescreen

> ⚠️ **AltStore-Signatur** läuft alle 7 Tage ab (kostenlose Apple ID).  
> AltStore erneuert sie automatisch wenn iPhone & PC im selben WLAN sind.

---

## Häufige Fehler & Lösungen

| Problem | Lösung |
|---|---|
| Build schlägt fehl mit "No such file" | Prüfe ob `index.html` im richtigen Ordner liegt |
| Icon fehlt | `icon-1024.png` muss exakt so heißen und 1024×1024 px sein |
| App crasht beim Start | Prüfe ob `index.html` valides HTML ist |
| AltStore zeigt "Invalid IPA" | IPA nochmals frisch herunterladen und entpacken |

---

## App aktualisieren

Wenn du `index.html` änderst:
1. Neue `index.html` auf GitHub hochladen (Datei ersetzen)
2. Actions → **Run workflow**
3. Neue IPA herunterladen und über AltStore neu installieren

---

## Technische Details

- Die iOS-App ist ein **WKWebView-Wrapper** – deine HTML-Datei läuft komplett lokal
- `localStorage` funktioniert ✅
- Externe URLs (dein In-App-Browser) funktionieren ✅ (NSAllowsArbitraryLoads aktiviert)
- Status-Bar und Home-Indicator werden ausgeblendet für maximalen Platz
- Unterstützt Portrait & Landscape
