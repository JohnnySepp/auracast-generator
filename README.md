# Auracast™ QR Generator

A browser-based generator for Auracast™ Broadcast Audio URI (BAU) QR codes — 100% client-side, no backend required. Fully responsive.

> **[🔗 Live Demo](https://auracast-generator.de/)**

---

## Features

- Generates standards-compliant **BAU v1.0** QR codes for Auracast™ streams
- **Broadcast Encryption** toggle — automatically sets `AT:1` (public) or `AT:2` (encrypted)
- **Advanced settings** — Bluetooth MAC address (`AD`), audio channels (`AS`) and audio quality (`SQ`)
- **Logo overlay** — none, Auracast™ icon, or custom upload
- **Print preview** with two templates and optional privacy notice
- **Assistive Hearing print template** — A4 poster with the International Symbol for Deafness, stream name and broadcast code on a blue background
- **Fullscreen view** (`view=fullscreen`) for room displays — dark background, optional custom background image
- **Copy QR code** — copies the QR code image directly to the clipboard
- **Save QR code** — downloads the QR code as a PNG file named after the stream
- **Share link** — copies a pre-configured URL that opens the fullscreen view directly
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
7. Use the action buttons:
   - **Print preview** — opens a modal with print template selection and optional privacy notice
   - **Fullscreen** — opens a full-screen display mode for room displays
   - **Save QR code** — downloads the QR code as `<streamname>.png`
   - **Copy QR code** — copies the QR code as a PNG image to the clipboard
   - **Share link** — copies a URL that pre-fills all fields and opens the fullscreen view directly

---

## Print Preview

The print preview modal offers two templates selectable via radio buttons:

**No template** — centered QR code with stream name, description and broadcast code on a white background. Optimized for A4 portrait.

**Assistive Hearing** — full A4 poster in blue (`#17128b`) with:
- The International Symbol for Deafness (large, white)
- QR code with scan hint
- Stream name and broadcast code in a white access box

Both templates support an optional **privacy notice** (checkbox, visible for Assistive Hearing only):
> Hearing assistance via Auracast™ Bluetooth Broadcast is available in this room. The audio signal is transmitted wirelessly and can be received with compatible Auracast™-enabled devices or hearing aids. No data or audio signals are stored or further processed.

The privacy notice is available in DE and EN and switches with the language toggle.

---

## Share Link

The **Share link** button always generates a URL with `view=fullscreen` appended. This means the recipient opens the fullscreen view directly — ideal for:

- Bookmarks on room display PCs
- Automated links from Node-RED or room control systems
- Sharing the exact configuration with colleagues

**Example of a generated share link:**
```
auracast-qr-generator.html?name=Lecture+Hall+A&mac=AABBCC112233&logo=auracast&view=fullscreen
```

---

## URL Parameters

All fields can be pre-filled via GET parameters. This makes it easy to share pre-configured links or automate QR code generation from a room control system or workflow tool (e.g. via Node-RED).

> **Security note:** GET parameters are visible in the URL and may be stored in browser history, server logs, and shared links. For a broadcast code that functions more like a password, this is a deliberate trade-off: the openness of GET parameters is intentional here, as it enables easy sharing of pre-configured links — for example, a direct link to the fullscreen view that room staff can bookmark or trigger automatically. If the broadcast code is sensitive, consider whether sharing it via URL is appropriate for your use case.

| Parameter | Value | BAU field | Description |
|---|---|---|---|
| `name` | text | `BN` (Base64) | Stream name — required to generate QR code |
| `desc` | text | — | Description / additional info (display only) |
| `pwd` | text | `BC` (Base64) | Password — also activates the encryption toggle |
| `mac` | `AABBCC112233` (no colons) | `AD` (12-char hex) | Bluetooth MAC address of the sender |
| `audio` | `mono` / `stereo` | `AS:1` / `AS:2` | Audio channel count |
| `qual` | `standard` / `high` | `SQ:0` / `SQ:1` | Broadcast audio quality |
| `logo` | `none` / `auracast` | — | Logo overlay selection |
| `view` | `modal` / `fullscreen` | — | Open print preview or fullscreen view on load |

> **Note on `mac`:** The MAC address must be passed **without colons** as a 12-character hex string (e.g. `mac=AABBCC112233`). If you have a MAC address in the format `AA:BB:CC:11:22:33`, simply remove the colons before using it as a URL parameter.

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

The generated QR code contains a **Broadcast Audio URI (BAU)** as defined by the Bluetooth SIG. The `;;` at the end is the required terminator per the BAU v1.0 specification.

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

## Customization

The generator supports optional asset files for per-deployment customization without modifying the HTML.

Place the following file in an `assets/` folder next to the HTML file:

| File | Description |
|---|---|
| `assets/bg.jpg` | Background image for the fullscreen view. If present, displayed with a dark overlay (`rgba(13,13,13,0.75)`). Falls back to solid `#0d0d0d` if not found. |

**Example folder structure:**
```
auracast-qr-generator.html
assets/
  bg.jpg
```

> Asset files are detected at runtime via a lightweight image probe — no configuration required.

---

## Credits & License

### Content & Documentation
© 2026 Sebastian Stake — licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

### Code
Licensed under the [MIT License](LICENSE)

### References
- [Bluetooth SIG — BAU v1.0 Specification](https://www.bluetooth.com/specifications/specs/broadcast-audio-uniform-resource-identifier/)
- [Auracast™ | Bluetooth® Technology Website](https://www.bluetooth.com/auracast/)
- Auracast™ icon via [homarr-labs/dashboard-icons](https://github.com/homarr-labs/dashboard-icons)
- International Symbol for Deafness via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:International_Symbol_for_Deafness.svg) (public domain)

---

> Auracast™ is a registered trademark of Bluetooth SIG, Inc. All brand names are the property of their respective owners.
