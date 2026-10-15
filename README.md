# Stunden – Arbeitszeit

Web-App fürs Handy zur Arbeitszeiterfassung einer Firma:

- Der Chef richtet die App einmal ein (Benutzer z. B. „ChäsLand“) und eröffnet die Mitarbeiter.
- Jeder Mitarbeiter wählt in der App seinen Namen und gibt einmal pro Gerät seinen 4-stelligen PIN ein. Mehrere Geräte sind möglich.
- Beim Öffnen erscheint immer die aktuelle Woche mit der Eingabe für heute.
- Abrechnung pro Woche gegenüber 45 Std., Saldo bis Ende der letzten fertig eingetragenen Woche
- Ferien (25 Tage), Krankheit, Unfall, Feiertage Kanton Thurgau inkl. 1. Mai
- Der Chef sieht alle Mitarbeiter mit Saldo und kann Einträge korrigieren (beim Tag markiert)
- Alle Geräte zeigen dieselben Daten, Änderungen erscheinen sofort überall
- Monatsblatt als PDF, tägliche Kopie aller Daten

## 1. Firebase (einmalig)

Firebase-Projekt „Stundenerfassung“ (bereits erstellt):

1. **Authentication → Anmeldemethode**: **Anonym** und **E-Mail/Passwort** aktiviert.
2. **Firestore Database → Regeln**: den ganzen Inhalt durch `firestore.rules` ersetzen → **Veröffentlichen**. Bei jeder neuen Version dieser Datei wiederholen.
3. **Authentication → Einstellungen → Autorisierte Domains**: die Vercel-Adresse eintragen (ohne `https://`).

## 2. GitHub und Vercel

Alle Dateien dieses Ordners ins Repository laden (bestehende ersetzen). Vercel veröffentlicht automatisch neu.

## 3. Einrichten (Chef, einmalig)

1. Die Vercel-Adresse am PC öffnen. Es erscheint **Stunden einrichten**.
2. Benutzer **ChäsLand**, E-Mail **c.aeschbacher@gmail.com** und ein Passwort (mind. 6 Zeichen) → **Einrichten**.
3. Unter **Mitarbeiter verwalten** den Mitarbeiter eröffnen, z. B. „Adrian Tritten“, Beginn **13.01.2026**. Die App erstellt dabei den PIN.
4. Oben bei der Auswahl „Adrian Tritten“ wählen → **Einstellungen** → **Sicherung wiederherstellen** → `Stunden_Import_Jan-Sept_2026.json` wählen. Damit sind alle Stunden seit Januar drin.

**Chef auf weiteren PCs oder Handys:** Adresse öffnen → **Als Chef anmelden** → Benutzer „ChäsLand“ (oder die E-Mail-Adresse) und Passwort. Das geht auf beliebig vielen Geräten gleichzeitig.

**Passwort vergessen:** bei der Anmeldung antippen, der Link kommt an die E-Mail-Adresse des Chefs.

**E-Mail-Adresse ändern:** Übersicht → Einstellungen → E-Mail-Adresse ändern. Es kommt ein Bestätigungslink an die neue Adresse. Nach dem Antippen gilt sie, der Benutzer „ChäsLand“ bleibt gleich.

## 4. Mitarbeiter-Handy (einmalig)

1. Die Vercel-Adresse in **Safari** öffnen → Teilen → **Zum Home-Bildschirm**.
2. Die App öffnen → **Wer bist du?** → eigenen Namen antippen → **PIN** eingeben.

Ab dann öffnet die App immer direkt den heutigen Tag. Den PIN sieht der Chef in der Übersicht unter **Mitarbeiter verwalten**.

**Neues oder zweites Handy:** einfach wieder Namen wählen und PIN eingeben. Die Daten sind sofort da.

**Handy verloren:** Der Chef tippt beim Namen auf **Geräte trennen** und danach auf **Neuer PIN**.

## Saldo

Abgerechnet wird die Wochensumme gegenüber 45 Std. Freie Werktage müssen nicht eingetragen werden.
Jeder Ferien-, Kranken- oder Unfalltag senkt das Wochensoll um 9 Std. (halber Tag 4:30), egal ob Montag oder Samstag. Ein Feiertag von Mo–Fr ebenso. Eine ganze Ferienwoche ergibt Saldo 0. „Ist“ sind immer nur die gearbeiteten Stunden.
Arbeit an einem Feiertag zählt zusätzlich (Kompensation). Ein freier Samstag ohne Ferien braucht keinen Eintrag; die Woche muss trotzdem 45 Std. ergeben.
Der angezeigte Saldo gilt bis Ende der letzten fertig eingetragenen Woche. Eine Woche gilt als fertig, wenn sie vorbei ist und bis Freitag oder Samstag eingetragen ist, oder wenn schon spätere Tage eingetragen sind. Die laufende Woche zählt erst, wenn sie fertig ist.
Ist eine vergangene Woche noch ganz leer, zeigt die App oben einen Hinweis.

## Chef

- **Übersicht alle**: alle Mitarbeiter mit Saldo, Wochenstunden, Restferien und offenen Tagen.
- Oben einen Mitarbeiter wählen: dieselbe Ansicht wie auf seinem Handy. Korrekturen sind beim Tag mit „Korrigiert vom Chef“ markiert, im Monatsblatt mit „(korrigiert)“.
- Arbeitszeit, Ferien, Feiertage und Beginn pro Mitarbeiter unter **Einstellungen** (Mitarbeiter vorher oben wählen). Mitarbeiter sehen diese Werte nur.
- **Abmelden** unter Einstellungen.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | die ganze App |
| `firebase-config.js` | Zugangsdaten des Firebase-Projekts |
| `firestore.rules` | Sicherheitsregeln für Firestore |
| `sw.js`, `manifest.webmanifest`, Icons | App-Symbol und Start ohne Netz |
