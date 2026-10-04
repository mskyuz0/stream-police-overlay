# STF Tactical HUD - Police Streaming Overlay

Browser source overlay for OBS streaming with cyber police theme - San Andreas Law Enforcement (GTA V RP). Integrated live chat via Social Stream Ninja, real-time tactical status, and futuristic military HUD.

<p align="center">
  <img src="https://media.discordapp.net/attachments/1531751267859169573/1556376367736553704/image.png?backend=b2&ex=6ac3eff2&is=6ac29e72&hm=a3db8aa6236289ce42dc25fdf05c239ac49eaedada1ddcf890f3a1ae7b7aa046&=&format=webp&quality=lossless&width=1024&height=575" width="600" alt="STF Tactical HUD Preview">
</p>

<p align="center">
  <strong>Navy + Cyan + Red</strong> • <strong>Tactical HUD</strong> • <strong>Responsive 16:9</strong>
</p>

---

## Features

✓ **Topbar Header** – Operator name, callsign, rank, badge, mission in real-time  
✓ **Live Chat Panel** – Social Stream Ninja WebSocket multi-platform integration  
✓ **Chat Badges** – MOD (red), SUB (blue), VIP (yellow), OWNER (gold)  
✓ **Platform Colors** – YouTube (red), TikTok (cyan), Twitch (purple), Other (default)  
✓ **Tactical Status** – 6-row monitoring: network, bodycam, GPS, encryption, link, radio  
✓ **Real-time Clock** – Auto-detect local timezone, updates every second  
✓ **Bodycam REC** – Smooth red fade blinking animation  
✓ **Demo Mode** – Testing without SSN session  
✓ **Responsive Scale** – vw/vh units, fits 1920x1080 down to 1280x720  
✓ **Transparent Background** – OBS Browser Source compatible  
✓ **Smooth Animations** – Chat fade-in + slide-up, bodycam pulse, scanner effects  

---

## Prerequisites

- OBS Studio 28+
- Social Stream Ninja extension/app (for live mode)
- Modern browser (Chrome, Firefox, Edge)

---

## Installation

### Option A: Local File (Offline Testing)

1. Clone or download this repo
2. Open in OBS Browser Source:
   - **URL:** `file:///path/to/index.html?demo=1`
   - **Width:** 1920
   - **Height:** 1080
   - **FPS:** 30

Demo will display fake chat messages every 8 seconds.

### Option B: Local Server (Recommended)

For SSN WebSocket, use local HTTP server:

**Python 3:**
```bash
cd stream-police-overlay
python3 -m http.server 8000
```

**Node.js:**
```bash
npx http-server . -p 8000
```

Then in OBS:
- **URL:** `http://localhost:8000/index.html?demo=1`
- Keep terminal running while streaming

---

## Configuration

### Query Parameters

| Parameter | Example | Function |
|-----------|---------|----------|
| `op` | `?op=WESLEY%20MORRIETT` | Operator name |
| `call` | `?call=APEX-09` | Callsign/class |
| `rank` | `?rank=SERGEANT` | Rank |
| `badge` | `?badge=122` | Badge number |
| `mission` | `?mission=TEXT` | Mission statement |
| `tz` | `?tz=7` | Timezone offset (GMT+7) |
| `rec` | `?rec=0` | Disable REC indicator |
| `demo` | `?demo=1` | Demo mode (fake chat) |
| `ssn` | `?ssn=SESSION_ID` | SSN session for live |
| `limit` | `?limit=30` | Max chat messages |

### Example URLs

**Offline testing:**
```
file:///D:/Workspace/WEB/stream-police-overlay/index.html?demo=1&op=KYU&call=APEX-09
```

**Live with SSN:**
```
http://localhost:8000/index.html?ssn=abc123xyz&op=WESLEY%20MORRIETT&call=APEX-09&tz=7
```

**Custom timezone (Jakarta WIB):**
```
http://localhost:8000/index.html?demo=1&tz=7
```

---

## Live Chat Integration (SSN)

### 1. Setup Social Stream Ninja

1. Install Social Stream Ninja extension/app
2. Add platform: YouTube, TikTok, Twitch, or Facebook
3. Open SSN settings → **Mechanics**
4. Enable both checkboxes:
   - ✅ Enable remote API control of extension
   - ✅ Send chat messages to API server
5. Restart SSN if prompted

### 2. Get Session ID

1. Open SSN dock in browser
2. Check URL: `https://socialstream.ninja/dock.html?session=[SESSION_ID]`
3. Copy `[SESSION_ID]`

### 3. Add to OBS

**URL with SSN:**
```
http://localhost:8000/index.html?ssn=YOUR_SESSION_ID&op=KYU&call=APEX-09
```

**Properties:**
| Field | Value |
|-------|-------|
| URL | URL above |
| Width | 1920 |
| Height | 1080 |
| FPS | 30 |
| Shutdown when not visible | ✓ |
| Refresh when scene active | ✓ |

### 4. Verify

Send chat on YouTube/TikTok/Twitch. Message should appear within 1-2 seconds in the overlay.

---

## Data Structure

### Chat Message SSN Payload

```json
{
  "chatname": "username",
  "chatmessage": "hello sergeant",
  "badge": "mod",
  "platform": "youtube",
  "isowner": false,
  "type": "youtube"
}
```

**Recognized Badges:**
- `mod` → MOD (red)
- `subscriber` → SUB (blue)
- `vip` → VIP (yellow)
- `verified` → VERIFIED (cyan)
- `isowner: true` → OWNER (gold)

**Platforms:**
- `youtube` → Username red (#ff6b6b)
- `tiktok` → Username cyan (#00f0f0)
- `twitch` → Username purple (#a970ff)
- Others → Username default cyan

---

## Layout

```
┌─────────────────────────────────────────────────────────┐
│ STF │ ORG │ OPERATOR │ CLASS │ RANK │ BADGE │ MISSION │ │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  ┌──────────────┐                 ┌──────────────────┐   │
│  │ CHAT PANEL   │                 │ TACTICAL STATUS  │   │
│  │ @user MOD    │                 │ NETWORK: CONN    │   │
│  │ hello sergeant│                 │ BODYCAM: REC ●   │   │
│  │ @user2 SUB   │                 │ GPS: PRECISION   │   │
│  │ nice overlay │                 │ ENCRYPT: AES-256 │   │
│  └──────────────┘                 │ LINK: SECURE     │   │
│                                    │ RADIO: ENCRYPTED │   │
│                                    └──────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## CSS Variables (Customization)

Edit file to change colors:

```css
:root {
  --cy: #1ca9d6;        /* Primary cyan */
  --cy2: #38c6e8;       /* Light cyan */
  --mut: #6b8495;       /* Muted */
  --mut2: #8da6b5;      /* Light muted */
  --red: #ff3f4f;       /* Warning red */
  --grn: #20d6a0;       /* Success green */
  --tx: #dceaf2;        /* Text color */
  --panel: rgba(6,17,28,.78); /* Panel bg */
}
```

---

## Troubleshooting

### Chat not showing in OBS but works in browser

- Make sure using `http://localhost` (not `file://`)
- Refresh Browser Source in OBS (click refresh icon)
- Check browser console (F12) for WebSocket errors
- Test with `?demo=1` first

### Demo mode chat not appearing

- Open URL with `?demo=1`
- F12 → Console, check for errors
- Refresh page

### Status stuck on STANDBY (live mode)

1. Match Session ID exactly
2. Enable both API toggles in SSN settings
3. Restart SSN
4. Test with `?demo=1` first to verify overlay health

### Emoji displays too large or weird

- Unicode emoji pass-through directly, inherits chat font-size
- If still large, edit CSS `.msg .txt { font-size: ... }`

### Wrong timezone

- Timezone auto-detected from OS
- Override with `?tz=7` (for GMT+7 WIB)
- Format: `?tz=-5` for GMT-5, `?tz=0` for GMT

---

## Development

### File Structure

```
stream-police-overlay/
├── index.html          # Single-file overlay (HTML + CSS + JS)
├── README.md           # Documentation
└── .git/               # Version control
```

### Tech Stack

- **HTML5** – Semantic markup
- **CSS3** – Animations, gradients, clip-path
- **Vanilla JS** – WebSocket, DOM rendering, timezone handling
- **Google Fonts** – Rajdhani, Share Tech Mono

### Modifications

Edit `HUD_CONFIG` object in `<script>` section for:
- Default operator name
- Tactical status rows
- Badge styles
- Platform colors

---

## Streaming Tips

1. **Position in OBS:** Place overlay above gameplay layer
2. **Size:** Default 1920x1080, scale down if needed
3. **Alerts:** Chat panel auto-scrolls to latest message
4. **Performance:** Vanilla JS no framework, minimal CPU usage
5. **Mobile viewers:** Responsive layout, works at various resolutions
6. **Multi-platform:** SSN merges YouTube + TikTok chat in real-time

---

## License

Free to use & modify for GTA V RP streaming.

For Social Stream Ninja integration, follow SSN GPL-3.0 license:
- Discord: https://discord.socialstream.ninja
- GitHub: https://github.com/steveseguin/social_stream

---

## Support

**Issues or suggestions?**

Create an issue on GitHub or contact directly.

**Contributions welcome** – fork, modify, submit PR.

---

**Last updated:** October 2026  
**Status:** Production Ready ✓
