# Auracast™ QR Generator

A browser-based generator for Auracast™ Broadcast Audio URI (BAU) QR codes — 100% client-side, no backend required.

> **[🔗 Live Demo](https://johnnysepp.github.io/auracast-generator/auracast-qr-generator.html)**

---

## Features

- Generates standards-compliant **BAU v1.0** QR codes for Auracast™ streams
- **Broadcast Encryption** toggle — automatically sets `AT:1` (public) or `AT:2` (encrypted)
- **Advanced settings** — Bluetooth MAC address, audio channels and audio quality
- **Logo overlay** — none, Auracast™ icon, or custom upload
- **Fullscreen view** (`view=fullscreen`) for room displays
- **Print preview** with browser print dialog and optimized A4 layout
- **URL parameters** for automated pre-filling (e.g. via Node-RED)
- **DE / EN** language support with automatic browser language detection
- Dark mode support

---

## Usage

1. Enter the Auracast™ stream name.
2. Optionally add a description (e.g. language or event title).
3. Enable **Broadcast Encryption** and enter a password if the stream is encrypted.
4. Optionally expand **Advanced settings** to set the Bluetooth MAC address, audio channels and quality.
5. The QR code is generated automatically.
6. Choose a logo overlay if desired.
7. Use **Print preview** to print or save as a sign, or **Fullscreen** for room display.

---

## URL Parameters

All fields can be pre-filled via GET parameters. This is useful for automated workflows (e.g. from a room control system or Node-RED).

| Parameter | Value | BAU field | Description |
|---|---|---|---|
| `name` | text | `BN` (Base64) | Stream name — required to generate QR code |
| `desc` | text | — | Description / additional info (display only) |
| `pwd` | text | `BC` (Base64) | Password — also activates the encryption toggle |
| `mac` | `AA:BB:CC:11:22:33` | `AD` (12-char hex) | Bluetooth MAC address of the sender |
| `audio` | `mono` / `stereo` | `AS:1` / `AS:2` | Audio channel count |
| `qual` | `standard` / `high` | `SQ:0` / `SQ:1` | Broadcast audio quality |
| `logo` | `none` / `auracast` | — | Logo overlay selection |
| `view` | `modal` / `fullscreen` | — | Open print preview or fullscreen view on load |

**Example — public stream:**
```
auracast-qr-generator.html?name=Lecture+Hall+A&desc=Simultaneous+Interpretation&mac=AABBCC112233
```

**Example — encrypted stream, stereo, open fullscreen directly:**
```
auracast-qr-generator.html?name=Conference+Room+3&pwd=Pa$$wor6&mac=AABBCC112233&audio=stereo&qual=high&logo=auracast&view=fullscreen
```

---

## BAU Payload Format

The generated QR code contains a **Broadcast Audio URI (BAU)** as defined by the Bluetooth SIG:

```
BLUETOOTH:UUID:184F;BN:<Base64>;AT:<1|2>[;AD:<HEX>][;BC:<Base64>][;AS:<1|2>][;SQ:<0|1>];;
```

| BAU field | Description | Values |
|---|---|---|
| `BN` | Stream name, Base64-encoded | — |
| `AT` | Announcement type | `1` = public, `2` = encrypted |
| `AD` | Bluetooth MAC address | 12-char hex, no colons |
| `BC` | Broadcast code / password, Base64-encoded | — |
| `AS` | Audio stream count | `1` = mono, `2` = stereo |
| `SQ` | Standard quality broadcast | `0` = standard, `1` = high quality |

**Example:**
```
BLUETOOTH:UUID:184F;BN:TGVjdHVyZSBIYWxsIEE=;AT:2;AD:AABBCC112233;BC:UGEkJHdvcjY=;AS:2;SQ:1;;
```

For the full specification, see the [Bluetooth SIG — Broadcast Audio URI (BAU v1.0)](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/).

---

## Credits & License

### Content & Documentation
© 2025 Sebastian Stake — licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

### Code
Licensed under the [MIT License](LICENSE)

### References
- [Bluetooth SIG — BAU v1.0 Specification](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/)
- [Auracast™ | Bluetooth® Technology Website](https://www.bluetooth.com/auracast/)
- Auracast™ icon via [homarr-labs/dashboard-icons](https://github.com/homarr-labs/dashboard-icons)

---

> Auracast™ is a registered trademark of Bluetooth SIG, Inc. All brand names are the property of their respective owners.
