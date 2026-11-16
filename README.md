# LiDAR-Aufmaß – iPhone-App für Raumaufmaße

Kostenlose, eigene iOS-App, die den LiDAR-Scanner (iPhone 12 Pro und neuer, also auch iPhone 14 Pro) über Apples **RoomPlan** nutzt.

## Was die App kann
- Räume live scannen, mit Apples Anleitung auf dem Bildschirm
- Automatische Erkennung von Wänden, Türen, Fenstern, Wandöffnungen und Möbeln
- **Aufmaß-Auswertung:** Grundfläche, Umfang, mittlere Raumhöhe, Volumen,
  Wandflächen brutto/netto (abzüglich Öffnungen), Fenster mit Brüstungshöhe
- **2D-Grundriss** mit nummerierten Wänden und Wandlängen
- **Export:** PDF (A4-Aufmaßblatt), CSV (öffnet direkt in deutschem Excel),
  USDZ-3D-Modell (z. B. für SketchUp, Blender, Revit/Archicad über Konverter)
- Alle Scans werden auf dem iPhone gespeichert und erscheinen auch in der Dateien-App

## Was du brauchst (alles kostenlos)
- einen Mac mit **Xcode** (gratis im Mac App Store) **oder** einen Windows-PC + kostenloses GitHub-Konto (siehe „Ohne Mac“)
- eine normale **Apple-ID** (kein bezahlter Entwickler-Account nötig)
- dein iPhone 14 Pro und ein USB-Kabel

> Mit kostenloser Apple-ID läuft die App **7 Tage**. Danach einfach das iPhone anschließen
> und in Xcode erneut auf ▶︎ klicken; die Scans bleiben erhalten.
> (Mit dem Apple Developer Program für 99 €/Jahr wäre sie 1 Jahr gültig, nötig ist das aber nicht.)

## Ohne Mac: Bauen in der Cloud (GitHub) + Installieren mit Sideloadly
1. Kostenloses Konto auf github.com anlegen → **New repository** (z. B. `lidar-aufmass`, *Public*
   ist am einfachsten, da macOS-Build-Minuten dann kostenlos sind).
2. Den **Inhalt** dieses Ordners hochladen (auch den versteckten Ordner `.github`!
   Am einfachsten über GitHub Desktop oder „Add file › Upload files“ und den Ordner hineinziehen).
3. Im Repo auf **Actions › Build IPA › Run workflow**. Nach ca. 5–10 Minuten unten bei
   „Artifacts“ `LidarAufmass-ipa` herunterladen und entpacken → `LidarAufmass.ipa`.
4. Auf dem Windows-PC **Sideloadly** installieren (sideloadly.io) – dafür werden iTunes und
   iCloud in der Version von apple.com benötigt (nicht aus dem Microsoft Store).
5. iPhone per USB anschließen, `.ipa` in Sideloadly ziehen, Apple-ID eingeben, **Start**.
6. Auf dem iPhone: Entwicklermodus einschalten und dem Profil vertrauen (siehe Schritt 7 und 9 unten).
7. Nach 7 Tagen: in Sideloadly einfach erneut installieren.

## Einrichtung mit Mac – Variante A: von Hand in Xcode
1. Xcode öffnen → **File › New › Project… › iOS › App**
   - Product Name: `LidarAufmass`, Interface: **SwiftUI**, Language: **Swift**
2. Die automatisch erzeugten Dateien `ContentView.swift` und `LidarAufmassApp.swift`
   löschen (Move to Trash).
3. Alle Dateien aus dem Ordner `Sources/` in das Projekt ziehen
   (Haken bei „Copy items if needed“ und beim Target).
4. Target **LidarAufmass › General › Minimum Deployments:** iOS **17.0**
5. Target **› Info** › mit dem „+“ hinzufügen:
   - `Privacy - Camera Usage Description` → „Wird zum Aufmessen von Räumen benötigt.“
   - optional, damit die Scans in der Dateien-App sichtbar sind:
     `Application supports iTunes file sharing` = YES und
     `Supports opening documents in place` = YES
6. Target **› Signing & Capabilities** › Team: **Add Account…** → mit Apple-ID anmelden →
   „(Dein Name) (Personal Team)“ wählen. Bundle Identifier eindeutig machen,
   z. B. `de.deinname.lidaraufmass`.
7. iPhone per Kabel anschließen und auf dem iPhone den **Entwicklermodus** einschalten:
   *Einstellungen › Datenschutz & Sicherheit › Entwicklermodus* (Neustart nötig).
8. Oben in Xcode dein iPhone als Ziel wählen → **▶︎ Run**.
9. Beim ersten Start auf dem iPhone: *Einstellungen › Allgemein › VPN & Geräteverwaltung*
   → deinem Entwicklerprofil **vertrauen**.

## Einrichtung – Variante B: mit XcodeGen
```bash
brew install xcodegen
cd LidarAufmass
xcodegen
open LidarAufmass.xcodeproj
```
Danach nur noch Schritt 6–9 von oben (Team wählen, Bundle-ID anpassen, Run).

## Tipps für gute Scans
- Gutes Licht, langsam gehen, das iPhone etwa auf Brusthöhe halten
- Alle Ecken und jede Wand einmal komplett erfassen, Fenster und Türen direkt anvisieren
- Spiegel und große Glasflächen verwirren LiDAR, dort ggf. nachmessen
- Ein Scan pro Raum (RoomPlan ist für Räume bis ca. 9 × 9 m ausgelegt)

## Genauigkeit
RoomPlan misst typischerweise auf einige Zentimeter genau. Für Angebote/Mengenermittlung
ist das meist gut brauchbar; für Maße, die passen müssen (Einbaumöbel, Fensterbestellung),
bitte mit Laser-Entfernungsmesser oder Zollstock kontrollieren.

## Falls Xcode meckert
- **Concurrency-/Actor-Fehler** (neue Xcode-Versionen): Target › Build Settings ›
  „Swift Language Version“ auf **Swift 5** stellen.
- **„Untrusted Developer“** auf dem iPhone: siehe Schritt 9.
- **Bundle-ID schon vergeben:** einfach eine andere, eigene ID eintragen.

## Mögliche Erweiterungen
- Mehrere Räume zu einem Geschossplan zusammenfügen (RoomPlan `StructureBuilder`)
- DXF-Export für CAD
- Manuelle Korrektur einzelner Maße in der App
