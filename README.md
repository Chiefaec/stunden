# Stunden – Arbeitszeit

Schlanke Web-App fürs iPhone zur Arbeitszeiterfassung:

- Arbeitszeiten pro Tag (Vormittag und Nachmittag), häufige Zeiten mit einem Tipp
- Abrechnung pro Woche gegenüber der Sollzeit (Standard 45 Std.), Jahressaldo ab 1.1.2026 bei 0
- Ferien (25 Tage), Krankheit, Unfall, Feiertage Kanton Thurgau inkl. 1. Mai
- Saldo und Restferien werden automatisch ins nächste Jahr übertragen
- Monatsblatt als PDF für den Vorgesetzten (Jahr → „Monatsblatt“)
- Hinweis auf nicht eingetragene Werktage und auf Tippfehler
- Speicherung online in Firebase (Firestore), funktioniert auch ohne Netz, plus eine tägliche Kopie

Die Daten liegen in einem eigenen Firebase-Projekt, getrennt von allen anderen Projekten.

## 1. Firebase-Projekt erstellen (einmalig, ca. 10 Minuten)

Alles auf console.firebase.google.com, mit demselben Google-Konto wie bisher:

1. **Projekt hinzufügen** → Name z. B. `stunden` → Google Analytics ausschalten → erstellen.
2. **Web-App registrieren:** auf der Projekt-Startseite das Symbol `</>` tippen → Name `Stunden` → registrieren (Firebase Hosting nicht ankreuzen). Das angezeigte `firebaseConfig`-Objekt kopieren.
3. Die Werte in `firebase-config.js` einfügen (die `HIER-EINFUEGEN`-Platzhalter ersetzen).
4. **Authentication** → „Jetzt starten“ → Sign-in method:
   - **Anonym** aktivieren
   - **E-Mail/Passwort** aktivieren
5. **Firestore Database** → „Datenbank erstellen“ → Standort **europe-west6 (Zürich)** → „Im Produktionsmodus starten“.
6. Firestore Database → **Regeln**: den ganzen Inhalt durch den Inhalt von `firestore.rules` ersetzen → **Veröffentlichen**.

Der kostenlose Spark-Plan reicht. Es ist keine Kreditkarte nötig.

## 2. Auf GitHub hochladen

1. Auf github.com ein neues Repository anlegen, zum Beispiel `stunden`.
2. Alle Dateien aus diesem Ordner hochladen („Add file“ → „Upload files“) und speichern.

## 3. Auf Vercel veröffentlichen

1. Auf vercel.com → „Add New…“ → „Project“ → das Repository importieren.
2. Framework Preset: **Other**. Build Command und Output Directory leer lassen → „Deploy“.
3. Die neue Adresse (z. B. `stunden-xyz.vercel.app`) in Firebase eintragen: **Authentication → Settings → Autorisierte Domains → Domain hinzufügen**.

Jede Änderung, die du später auf GitHub speicherst, veröffentlicht Vercel automatisch neu.

## 4. Auf dem iPhone installieren

1. Die Vercel-Adresse in **Safari** öffnen.
2. Teilen-Symbol → **Zum Home-Bildschirm**.
3. In der App → Einstellungen → **Konto sichern** → E-Mail und Passwort festlegen.

Ohne Schritt 3 funktioniert die App auch, die Daten sind dann aber an dieses eine Handy gebunden. Solange das Konto nicht gesichert ist, zeigt die App oben einen Hinweis.

## So wird gespeichert

- Die App meldet sich beim ersten Öffnen unsichtbar an. Es gibt keinen Anmeldebildschirm.
- Jede Änderung wird sofort gespeichert. Ohne Netz bleibt sie auf dem Handy und wird übertragen, sobald wieder Netz da ist.
- Einmal pro Tag legt die App zusätzlich eine vollständige Kopie ab: `stunden/<konto>/backups/JJJJ-MM-TT` (in der Firebase-Konsole unter Firestore Database einsehbar).
- **Neues Handy:** App öffnen → Einstellungen → „Ich habe schon ein Konto“ → mit E-Mail und Passwort anmelden.
- Ist längere Zeit nichts übertragen worden oder schlägt das Speichern fehl, erscheint oben ein roter Hinweis.

## Monatsblatt

Jahr → beim gewünschten Monat auf **Monatsblatt** tippen → im Druckfenster über das Teilen-Symbol als PDF sichern oder direkt drucken. Den Namen für das Blatt trägt man unter Einstellungen ein.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | die ganze App |
| `firebase-config.js` | Zugangsdaten des Firebase-Projekts (selbst ausfüllen) |
| `firestore.rules` | Sicherheitsregeln für Firestore |
| `sw.js`, `manifest.webmanifest`, Icons | App-Symbol und Start ohne Netz |
