# Auracast™ QR Generator

Ein browserbasierter Generator für Auracast™ Broadcast Audio URI (BAU) QR-Codes — vollständig clientseitig, kein Backend erforderlich.

> **[🔗 Live Demo](https://johnnysepp.github.io/auracast-generator/auracast-qr-generator.html)**

---

## Funktionen

- Generiert standardkonforme **BAU v1.0** QR-Codes für Auracast™-Streams
- **Broadcast Encryption**-Toggle — setzt automatisch `AT:1` (öffentlich) oder `AT:2` (verschlüsselt)
- **Bluetooth MAC-Adresse** wird als `AD`-Feld im BAU-Payload kodiert
- **Logo-Overlay** — ohne, Auracast™-Icon oder eigenes Bild
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
4. **Bluetooth MAC-Adresse** des Senders eingeben.
5. Der QR-Code wird automatisch generiert.
6. Bei Bedarf ein Logo-Overlay auswählen.
7. **Druckansicht** zum Ausdrucken oder als Aushang nutzen, oder **Vollbild** für die Raumanzeige.

---

## URL-Parameter

Alle Felder können per GET-Parameter vorausgefüllt werden. Dies ist für automatisierte Workflows nützlich (z.B. aus einem Raumsteuerungssystem oder Node-RED).

| Parameter | Wert | Beschreibung |
|---|---|---|
| `name` | Text | Sendername (erforderlich zur QR-Code-Generierung) |
| `desc` | Text | Beschreibung / Zusatzinfo |
| `pwd` | Text | Passwort — aktiviert auch den Encryption-Toggle |
| `mac` | `AA:BB:CC:11:22:33` | Bluetooth MAC-Adresse des Senders |
| `logo` | `none` / `auracast` | Logo-Overlay-Auswahl |
| `view` | `modal` / `fullscreen` | Druckansicht oder Vollbild direkt beim Laden öffnen |

**Beispiel — öffentlicher Stream:**
```
auracast-qr-generator.html?name=Hörsaal+A&desc=Simultanübersetzung&mac=AABBCC112233
```

**Beispiel — verschlüsselter Stream, direkt im Vollbild öffnen:**
```
auracast-qr-generator.html?name=Konferenzraum+3&pwd=Pa$$wor6&mac=AABBCC112233&logo=auracast&view=fullscreen
```

---

## BAU-Payload-Format

Der generierte QR-Code enthält einen **Broadcast Audio URI (BAU)** gemäß der Bluetooth SIG-Spezifikation:

```
BLUETOOTH:UUID:184F;BN:<Base64>;AT:<1|2>[;AD:<HEX>][;BC:<Base64>];;
```

| Feld | Beschreibung |
|---|---|
| `BN` | Sendername, Base64-kodiert |
| `AT` | Announcement Type: `1` = öffentlich, `2` = verschlüsselt |
| `AD` | Bluetooth MAC-Adresse (12-stellig hex, ohne Doppelpunkte) |
| `BC` | Broadcast Code / Passwort, Base64-kodiert |

**Beispiel:**
```
BLUETOOTH:UUID:184F;BN:SMO2cnNhYWwgQQ==;AT:2;AD:AABBCC112233;BC:UGEkJHdvcjY=;;
```

Die vollständige Spezifikation ist hier verfügbar: [Bluetooth SIG — Broadcast Audio URI (BAU v1.0)](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/).

---

## Danksagung & Lizenz

### Inhalt & Dokumentation
© 2025 Sebastian Stake — lizenziert unter [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

### Code
Lizenziert unter der [MIT-Lizenz](LICENSE)

### Referenzen
- [axute/auracast-bau-qr-code](https://github.com/axute/auracast-bau-qr-code) — browserbasierter BAU QR Generator (MIT)
- [Bluetooth SIG — BAU v1.0 Spezifikation](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/)
- [Auracast™ | Bluetooth® Technologie-Website](https://www.bluetooth.com/auracast/)
- Auracast™-Icon via [homarr-labs/dashboard-icons](https://github.com/homarr-labs/dashboard-icons)

---

> Auracast™ ist ein eingetragenes Markenzeichen der Bluetooth SIG, Inc. Alle Markennamen sind Eigentum ihrer jeweiligen Inhaber.
