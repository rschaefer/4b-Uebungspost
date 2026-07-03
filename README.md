# ÜbungsPost – Klasse 4b

Ein simulates E-Mail-System für den Grundschulunterricht. Kinder können Übungs-Mails schreiben und empfangen – ohne echten Versand, ohne echte Adressen, ohne Installation.

🌐 **Erreichbar unter:** https://rschaefer.github.io/4b-Uebungspost/

---

## Was ist ÜbungsPost?

ÜbungsPost simuliert einen E-Mail-Client im Browser. Kinder lernen:
- Wie eine E-Mail aufgebaut ist (Von, An, Cc, Bcc, Betreff, Nachricht)
- Wie man eine E-Mail schreibt und abschickt
- Was Cc (Kopie) und Bcc (Blindkopie) bedeuten
- Wie ein Posteingang und ein Gesendet-Ordner funktionieren

Es werden keine echten E-Mails verschickt. Alle Nachrichten bleiben innerhalb der App und werden in einer gesicherten Datenbank gespeichert.

---

## Technik im Überblick

| Komponente | Dienst | Kosten |
|---|---|---|
| Hosting der Webseite | GitHub Pages | kostenlos |
| Datenbank (Mails speichern & laden) | Google Firebase Firestore | kostenlos |
| Datenschutz | Nur Pseudonyme, keine echten Namen nötig, Server in der EU | ✅ |

---

## Funktionen

- **Login** mit Vorname → automatische Übungs-Adresse (`vorname@4b.de`)
- **Posteingang** mit Beispielmails und empfangenen Nachrichten
- **Ungelesene Mails** werden rot markiert und verschwinden nach dem Lesen
- **Neue Mail schreiben** mit An, Cc, Bcc und Betreff
- **Senden** speichert die Mail direkt in der Datenbank beim Empfänger
- **Synchronisieren** lädt neue Mails aus der Datenbank in den Posteingang
- **Gesendet**-Tab zeigt alle gesendeten Mails inklusive Cc und Bcc
- **Antworten** füllt Empfänger und Betreff automatisch aus

---

## Benutzung im Unterricht

### Vorbereitung (einmalig, durch Lehrkraft)

1. Seite aufrufen: `https://rschaefer.github.io/4b-Uebungspost/`
2. Sicherstellen, dass alle iPads mit dem WLAN verbunden sind
3. Auf den iPads Safari öffnen und den Link als Lesezeichen speichern
3. Alternativ den Link über einen QRCode zur Verfügung stellen

### Ablauf für die Kinder

1. **Seite öffnen** im Browser (Safari auf iPad)
2. **Vornamen eingeben** – die App erstellt daraus automatisch die Adresse (z.B. `thomas@4b.de`)
3. **Posteingang lesen** – drei Beispielmails warten bereits
4. **Neue Mail schreiben** – Tab „Neue Mail" antippen, Felder ausfüllen, „Senden" drücken
5. **Synchronisieren** – der Empfänger tippt auf „🔄 Synchronisieren", die neue Mail erscheint im Posteingang

### Wichtige Hinweise

- **Groß-/Kleinschreibung beim Namen:** Die App wandelt Namen automatisch um (`Thomas` → `thomas@4b.de`). Umlaute werden ersetzt (ä→ae, ö→oe, ü→ue, ß→ss).
- **Mehrere Empfänger** im An-, Cc- oder Bcc-Feld durch Komma trennen: `mia@4b.de, thomas@4b.de`
- **Bcc:** Der Empfänger erhält die Mail, aber kein anderer Empfänger sieht seine Adresse.
- **Ungelesene Mails** werden mit einem roten Punkt markiert und gelten als gelesen, sobald sie angeklickt werden.
- Der Lese-Status wird pro Gerät gespeichert – wechselt ein Kind das iPad, gelten alle Mails dort wieder als ungelesen.

---

## Änderungen vornehmen

### Beispielmails anpassen

In der Datei `index.html` ab Zeile 354 (Suche nach `const sampleMails`):

```javascript
const sampleMails = [
  {
    _docName: "sample-1",
    from: "lehrerin.frau-kirchmeier@4b.de",
    subject: "Willkommen bei ÜbungsPost! 🎉",
    body: "Hallo! ..."
  },
  // weitere Beispielmails hier ergänzen
];
```

### Firebase-Projekt ändern

In der Datei `index.html` ganz oben im Script-Block (Zeile 361):

```javascript
const FIREBASE_PROJECT_ID = "uebungspost-4b";
```

### Nach jeder Änderung

1. Datei `index.html` auf GitHub hochladen (Upload → Commit)
2. Unter `https://github.com/rschaefer/4b-Uebungspost/actions` den Fortschritt prüfen (grüner Haken = fertig)
3. Auf dem iPad: Einstellungen → Safari → „Verlauf und Websitedaten löschen" → Seite neu aufrufen

---

## Firebase Firestore – Sicherheitsregeln

Damit die App ohne Login lesen und schreiben kann, müssen folgende Regeln in der [Firebase Console](https://console.firebase.google.com) unter **Firestore → Regeln** eingetragen sein:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /mails/{mailId} {
      allow read, write: if true;
    }
  }
}
```

Diese Regeln erlauben allen Lesenden und Schreibenden Zugriff auf die Mails-Collection. Da keine echten personenbezogenen Daten gespeichert werden (nur Pseudonyme und Übungstexte), ist das für dieses Schulprojekt unbedenklich.

---

## Datenschutz

- Es werden **keine echten Namen** erfasst – nur frei wählbare Vornamen als Pseudonyme
- Es werden **keine Passwörter** gespeichert
- Die Datenbank liegt auf **Servern in der EU** (Firebase Region `eur3`)
- Die Webseite läuft auf GitHub-Servern (USA) – sie enthält jedoch keine personenbezogenen Daten, sondern nur die App selbst
- Die App ist ausschließlich für den internen Gebrauch im Klassenzimmer gedacht

---

*Erstellt im Juni 2026 als Gastprojekt für die Klasse 4b · ÜbungsPost ist ein Lernprojekt, keine echte E-Mail-Anwendung.*
