# points2plans

Statische Website für einen Scan-to-Plan / Scan-to-BIM-Service: Punktwolke (.ply, .obj) hochladen, IFC, DWG oder PDF herunterladen.

Kein Build-Schritt, keine Abhängigkeiten. Reines HTML, CSS und JavaScript.

```
index.html        Startseite mit Punktwolken-Viewer, Upload-Formular, Preisen
i18n.js           Übersetzungen Englisch und Spanisch
impressum.html    Impressum (Platzhalter ausfüllen!)
datenschutz.html  Datenschutzerklärung (Gerüst, ausfüllen lassen!)
legal.css         Styles der Rechtsseiten
favicon.svg       Icon
.nojekyll         sagt GitHub Pages, die Dateien unverändert auszuliefern
```

## Auf GitHub Pages veröffentlichen

1. Auf github.com ein neues Repository anlegen, z. B. `points2plans`.
2. Alle Dateien aus diesem Ordner hochladen (inklusive `.nojekyll`).
   Im Browser: „Add file → Upload files“. Oder im Terminal:
   ```bash
   git init && git add . && git commit -m "Erste Version" && git branch -M main
   git remote add origin https://github.com/DEIN-NAME/points2plans.git
   git push -u origin main
   ```
3. Im Repository: **Settings → Pages → Source: Deploy from a branch → main / (root)** → Save.
4. Nach ca. 1 Minute ist die Seite unter `https://DEIN-NAME.github.io/points2plans/` erreichbar.

### Eigene Domain: points2plans.com (gekauft bei INWX)

Die Datei `CNAME` mit `points2plans.com` liegt schon im Ordner.

1. Domain bei einem Registrar kaufen (z. B. INWX, Namecheap, Cloudflare).
2. DNS beim Registrar einstellen:
   - für `www.points2plans.eu`: CNAME auf `DEIN-NAME.github.io`
   - für `points2plans.eu` ohne www: A-Records auf
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
3. In **Settings → Pages → Custom domain** die Domain eintragen (GitHub legt dann eine `CNAME`-Datei an) und **Enforce HTTPS** aktivieren.

## Sprachen (DE / EN / ES)

Die Seite ist in `index.html` auf Deutsch geschrieben. `i18n.js` enthält zu jedem deutschen Text die englische und spanische Fassung.

- Die Sprache wird so gewählt: `?lang=en` in der Adresse, sonst die zuletzt gewählte Sprache, sonst die Browsersprache, sonst Deutsch.
- Direkte Links: `…/?lang=en`, `…/?lang=es`. Deutsch ist die Adresse ohne Zusatz.
- **Text ändern:** in `index.html` den deutschen Text ändern und in `i18n.js` den Schlüssel (links) genauso anpassen. Fehlt ein Eintrag, bleibt die Stelle deutsch.
- **Weitere Sprache:** Kürzel in `langs` ergänzen, in jedem Eintrag unter `text` eine Zeile hinzufügen, einen Block unter `dyn` und `meta` anlegen und in `index.html` einen Button im Bereich `class="lang"` ergänzen.
- Impressum und Datenschutz bleiben deutsch (für einen deutschen Anbieter ausreichend).
- Für Google: Weil die Übersetzung im Browser passiert, indexiert Google meist nur die deutsche Fassung. Wenn englische und spanische Suchtreffer wichtig werden, später je Sprache eine eigene Seite erzeugen (`/en/`, `/es/`).


## Versionen: 2D jetzt, 3D später

- **`main`** (live): nur 2D-Pläne als DWG und PDF, IFC/BIM erscheint als Kachel „Demnächst“.
- **`version-mit-bim`** (Branch auf GitHub): die vollständige Version mit IFC, LOD-Auswahl und 3D-Ansicht im Viewer. Nicht löschen.

**3D später wieder einschalten:** Am einfachsten auf GitHub einen Pull Request von `version-mit-bim` nach `main` öffnen und die Texte prüfen. Oder in `index.html` von `main` von Hand:
1. Den auskommentierten BIM-Button im Viewer wieder einkommentieren (`<!-- 3D später … -->`).
2. `const order = ['cloud','plan']` auf `['cloud','plan','bim']` ändern.
3. Upload-Formular, Preistabelle und Formate aus `version-mit-bim` übernehmen.

## Anfrage-Postfach (E-Mail mit Anhang)

Besucher schreiben über das Postfach eine Anfrage. Sie landet als E-Mail in Ihrem Postfach, mit Name, E-Mail-Adresse, Nachricht, Auswahl (Formate, Umfang, Maßstab, Fläche, Preis) und Anhang oder Download-Link.

- Versand über **FormSubmit** (formsubmit.co), kostenlos, ohne Konto.
- Anhänge: nur `.ply` / `.obj`, **zusammen höchstens 10 MB** (Grenze von FormSubmit). Größere Scans schicken Besucher als Link (WeTransfer, Dropbox, Google Drive).
- Antworten: Einfach in Gmail auf die E-Mail antworten, sie geht an die Adresse des Besuchers.

**Einrichtung (einmalig):**
1. Eine erste Anfrage an `https://formsubmit.co/IHRE-ADRESSE` senden. FormSubmit schickt daraufhin eine Aktivierungs-E-Mail.
2. In der E-Mail auf **Activate Form** klicken. Danach erhalten Sie eine Zeichenfolge (zufällig aussehender Text).
3. In `index.html` oben eintragen: `formEndpoint: "https://formsubmit.co/DIE-ZEICHENFOLGE"`.
   So steht Ihre E-Mail-Adresse nicht im öffentlichen Code.

Solange `formEndpoint` leer ist, zeigt das Postfach „noch nicht freigeschaltet“ an.

## Was bis zum echten Betrieb fehlt

### Muss vor dem Livegang erledigt sein
- **Impressum und Datenschutz ausfüllen.** In Deutschland Pflicht, auch ohne Upload. Alle blau markierten `[…]`-Stellen ersetzen.
- **Schriften selbst hosten.** Google Fonts direkt einzubinden wurde in Deutschland bereits abgemahnt (LG München, 2022). Die drei Schriften (Archivo, Inter Tight, JetBrains Mono) über https://gwfh.mranftl.com herunterladen, in einen Ordner `fonts/` legen und die `<link … fonts.googleapis.com>`-Zeilen durch `@font-face`-Regeln ersetzen.
- **Inhalte prüfen.** Preise, Lieferzeiten, „±30 mm“, „Frankfurt“, „Ende-zu-Ende verschlüsselt“ und „Löschung nach 30 Tagen“ sind Platzhalter aus dem Mockup. Nur stehen lassen, was wirklich stimmt.

### Für echte Aufträge (Backend)
- **Upload großer Dateien:** direkt vom Browser in einen Objektspeicher in der EU (z. B. Hetzner Object Storage, Cloudflare R2, AWS S3 Frankfurt) über signierte URLs und Multipart-Upload, damit 20-GB-Dateien abbrechen und weiterlaufen können.
- **Kleiner API-Server** (z. B. Node, Python oder Supabase / Firebase-Funktionen): legt Aufträge an, gibt Upload-URLs aus, speichert den Status.
- **Bezahlung:** z. B. Stripe Checkout (Karte, SEPA, Rechnung für Firmen).
- **E-Mails:** Auftragsbestätigung, Rückfragen bei Lücken im Scan, „Ihre Dateien sind fertig“ (z. B. Postmark, Brevo).
- **Kundenbereich:** Login per E-Mail-Link, Liste der Aufträge, Downloads per zeitlich begrenzten Links.
- **Interner Bereich** für die Bearbeitung: Aufträge sehen, Dateien abholen, Ergebnisse hochladen, Status setzen.

### Rechtliches für Geschäftskunden
- **AGB** mit Leistungsbeschreibung, Genauigkeit, Haftung und Nachbesserung.
- **Auftragsverarbeitungsvertrag (AVV)**, denn Scans von Wohnungen und Büros sind oft personenbezogen.
- **Widerrufsbelehrung**, falls auch Privatpersonen bestellen dürfen.

### Die eigentliche Leistung
- Wer modelliert? Ein eigenes Team, Freelancer oder eine Software (z. B. teilautomatische Wanderkennung). Daraus ergeben sich die Lieferzeiten und Preise auf der Seite.
- Beispieldateien zum Download (eine echte IFC, DWG und PDF) schaffen viel Vertrauen.

### Sinnvoll, aber nicht dringend
- Professionelles Gegenlesen der englischen und spanischen Texte, vor allem der Fachbegriffe.
- Vorschaubild für Social Media (`og:image`, 1200 × 630 px).
- Datenschutzfreundliche Statistik ohne Cookies (z. B. Plausible, Simple Analytics).
