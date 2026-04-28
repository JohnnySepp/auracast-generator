# Auracast™ QR Generator

Ein browserbasierter Generator für Auracast™ Broadcast Audio URI (BAU) QR-Codes — vollständig clientseitig, kein Backend erforderlich.

> **[🔗 Live Demo](https://johnnysepp.github.io/auracast-generator/auracast-qr-generator.html)**

---

## Funktionen

- Generiert standardkonforme **BAU v1.0** QR-Codes für Auracast™-Streams
- **Broadcast Encryption**-Toggle — setzt automatisch `AT:1` (öffentlich) oder `AT:2` (verschlüsselt)
- **Erweiterte Einstellungen** — Bluetooth MAC-Adresse, Audiokanäle und Audioqualität
- **Logo-Overlay** — ohne, Auracast™-Icon oder eigenes Bild
- **Druckvorlage „Assistive Hearing"** — A4-Poster mit dem Internationalen Symbol für Gehörlosigkeit, QR-Code, Streamname und Broadcast Code auf blauem Hintergrund
- **Optionaler Datenschutzhinweis** für die Assistive-Hearing-Vorlage (DE/EN)
- **Vollbildansicht** (`view=fullscreen`) für Raumdisplays
- **Druckansicht** mit Browser-Druckdialog und optimiertem A4-Layout
- **URL-Parameter** für automatisches Vorausfüllen (z.B. via Node-RED)
- **DE / EN** Sprachunterstützung mit automatischer Browser-Spracherkennung
- Dark Mode Unterstützung

---

## Verwendung

1. Auracast™-Sendernamen eingeben.
2. Optional eine Beschreibung ergänzen (z.B. Sprache oder Veranstaltungstitel).
3. **Broadcast Encryption** aktivieren und Passwort eingeben, falls der Stream verschlüsselt ist.
4. Optional **Erweiterte Einstellungen** öffnen und Bluetooth MAC-Adresse, Audiokanäle und Qualität festlegen.
5. Der QR-Code wird automatisch generiert.
6. Bei Bedarf ein Logo-Overlay auswählen.
7. **Druckansicht** öffnen, Druckvorlage wählen (Standard oder Assistive Hearing) und optional Datenschutzhinweis anfügen.
8. **Vollbild** für die Raumanzeige nutzen.

---

## URL-Parameter

Alle Felder können per GET-Parameter vorausgefüllt werden. Das ermöglicht das einfache Teilen vorkonfigurierter Links oder die Automatisierung der QR-Code-Generierung aus einem Raumsteuerungssystem oder Workflow-Tool (z.B. Node-RED).

> **Sicherheitshinweis:** GET-Parameter sind in der URL sichtbar und können im Browserverlauf, Server-Logs und geteilten Links gespeichert werden. Für einen Broadcast Code, der eher wie ein Passwort fungiert, ist das ein bewusster Kompromiss: Die Offenheit von GET-Parametern ist hier gewollt, da sie das einfache Teilen vorkonfigurierter Links ermöglicht — zum Beispiel einen direkten Link zur Vollbildansicht, den Raumverantwortliche bookmarken oder automatisch aufrufen können. Wenn der Broadcast Code sensibel ist, sollte abgewogen werden, ob eine Übertragung per URL für den jeweiligen Anwendungsfall geeignet ist.

| Parameter | Wert | BAU-Feld | Beschreibung |
|---|---|---|---|
| `name` | Text | `BN` (Base64) | Sendername — erforderlich zur QR-Code-Generierung |
| `desc` | Text | — | Beschreibung / Zusatzinfo (nur Anzeige) |
| `pwd` | Text | `BC` (Base64) | Passwort — aktiviert auch den Encryption-Toggle |
| `mac` | `AA:BB:CC:11:22:33` | `AD` (12-stellig hex) | Bluetooth MAC-Adresse des Senders |
| `audio` | `mono` / `stereo` | `AS:1` / `AS:2` | Anzahl der Audiokanäle |
| `qual` | `standard` / `high` | `SQ:0` / `SQ:1` | Audioqualität des Broadcasts |
| `logo` | `none` / `auracast` | — | Logo-Overlay-Auswahl |
| `view` | `modal` / `fullscreen` | — | Druckansicht oder Vollbild direkt beim Laden öffnen |

**Beispiel — öffentlicher Stream:**
```
auracast-qr-generator.html?name=Hörsaal+A&desc=Simultanübersetzung&mac=AABBCC112233
```

**Beispiel — verschlüsselter Stream, Stereo, direkt im Vollbild öffnen:**
```
auracast-qr-generator.html?name=Konferenzraum+3&pwd=Pa$$wor6&mac=AABBCC112233&audio=stereo&qual=high&logo=auracast&view=fullscreen
```

---

## BAU-Payload-Format

Der generierte QR-Code enthält einen **Broadcast Audio URI (BAU)** gemäß der Bluetooth SIG-Spezifikation:

```
BLUETOOTH:UUID:184F;BN:<Base64>;AT:<1|2>[;AD:<HEX>][;BC:<Base64>][;AS:<1|2>][;SQ:<0|1>];;
```

| BAU-Feld | Beschreibung | Werte |
|---|---|---|
| `BN` | Sendername, Base64-kodiert | — |
| `AT` | Announcement Type | `1` = öffentlich, `2` = verschlüsselt |
| `AD` | Bluetooth MAC-Adresse | 12-stellig hex, ohne Doppelpunkte |
| `BC` | Broadcast Code / Passwort, Base64-kodiert | — |
| `AS` | Anzahl der Audiostreams | `1` = Mono, `2` = Stereo |
| `SQ` | Standard Quality Broadcast | `0` = Standard, `1` = High Quality |

**Beispiel:**
```
BLUETOOTH:UUID:184F;BN:SMO2cnNhYWwgQQ==;AT:2;AD:AABBCC112233;BC:UGEkJHdvcjY=;AS:2;SQ:1;;
```

Die vollständige Spezifikation ist hier verfügbar: [Bluetooth SIG — Broadcast Audio URI (BAU v1.0)](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/).

---

## Danksagung & Lizenz

### Inhalt & Dokumentation
© 2025 Sebastian Stake — lizenziert unter [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

### Code
Lizenziert unter der [MIT-Lizenz](LICENSE)

### Referenzen
- [Bluetooth SIG — BAU v1.0 Spezifikation](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/)
- [Auracast™ | Bluetooth® Technologie-Website](https://www.bluetooth.com/auracast/)
- Auracast™-Icon via [homarr-labs/dashboard-icons](https://github.com/homarr-labs/dashboard-icons)
- Internationales Symbol für Gehörlosigkeit via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:International_Symbol_for_Deafness.svg) (Public Domain)

---

> Auracast™ ist ein eingetragenes Markenzeichen der Bluetooth SIG, Inc. Alle Markennamen sind Eigentum ihrer jeweiligen Inhaber.
