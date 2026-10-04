# STF Tactical HUD - Police Streaming Overlay

Browser source overlay untuk OBS streaming tema cyber police San Andreas Law Enforcement (GTA V RP). Integrasi live chat Social Stream Ninja, tactical status realtime, dan HUD militer futuristik.

<p align="center">
  <img src="https://media.discordapp.net/attachments/1531751267859169573/1556376367736553704/image.png?backend=b2&ex=6ac3eff2&is=6ac29e72&hm=a3db8aa6236289ce42dc25fdf05c239ac49eaedada1ddcf890f3a1ae7b7aa046&=&format=webp&quality=lossless&width=1024&height=575" width="600" alt="STF Tactical HUD Preview">
</p>

<p align="center">
  <strong>Navy + Cyan + Red</strong> • <strong>Tactical HUD</strong> • <strong>Responsive 16:9</strong>
</p>

---

## Fitur

✓ **Topbar Header** – Operator name, callsign, rank, badge, mission realtime  
✓ **Live Chat Panel** – Social Stream Ninja WebSocket integrasi multi-platform  
✓ **Chat Badges** – MOD (merah), SUB (biru), VIP (kuning), OWNER (emas)  
✓ **Platform Colors** – YouTube (merah), TikTok (cyan), Twitch (ungu), Other (default)  
✓ **Tactical Status** – 6 baris monitoring: network, bodycam, GPS, encryption, link, radio  
✓ **Clock Realtime** – Auto-detect timezone lokal PC, update setiap detik  
✓ **Bodycam REC** – Indikator blinking merah smooth fade animation  
✓ **Demo Mode** – Testing tanpa SSN session  
✓ **Responsive Scale** – vw/vh units, cocok 1920x1080 hingga 1280x720  
✓ **Transparent Background** – OBS Browser Source compatible  
✓ **Smooth Animations** – Chat fade-in + slide-up, bodycam pulse, scanner effects  

---

## Prasyarat

- OBS Studio 28+
- Social Stream Ninja extension/app (untuk live mode)
- Browser modern (Chrome, Firefox, Edge)

---

## Instalasi

### Opsi A: Local File (Offline Testing)

1. Clone atau download repo ini
2. Buka di OBS Browser Source:
   - **URL:** `file:///path/to/index.html?demo=1`
   - **Width:** 1920
   - **Height:** 1080
   - **FPS:** 30

Demo akan menampilkan fake chat messages setiap 8 detik.

### Opsi B: Local Server (Recommended)

Kalau menggunakan SSN WebSocket, gunakan HTTP server lokal:

**Python 3:**
```bash
cd stream-police-overlay
python3 -m http.server 8000
```

**Node.js:**
```bash
npx http-server . -p 8000
```

Kemudian di OBS:
- **URL:** `http://localhost:8000/index.html?demo=1`
- Biarkan terminal jalan saat streaming

---

## Konfigurasi

### Query Parameters

| Parameter | Contoh | Fungsi |
|-----------|--------|--------|
| `op` | `?op=WESLEY%20MORRIETT` | Nama operator |
| `call` | `?call=APEX-09` | Callsign/kelas |
| `rank` | `?rank=SERGEANT` | Rank |
| `badge` | `?badge=122` | Badge number |
| `mission` | `?mission=TEXT` | Mission statement |
| `tz` | `?tz=7` | Timezone offset (GMT+7) |
| `rec` | `?rec=0` | Disable REC indicator |
| `demo` | `?demo=1` | Demo mode (fake chat) |
| `ssn` | `?ssn=SESSION_ID` | SSN session untuk live |
| `limit` | `?limit=30` | Max chat messages |

### Contoh URL Lengkap

**Testing offline:**
```
file:///D:/Workspace/WEB/stream-police-overlay/index.html?demo=1&op=KYU&call=APEX-09
```

**Live dengan SSN:**
```
http://localhost:8000/index.html?ssn=abc123xyz&op=WESLEY%20MORRIETT&call=APEX-09&tz=7
```

**Custom timezone (Jakarta WIB):**
```
http://localhost:8000/index.html?demo=1&tz=7
```

---

## Integrasi Live Chat (SSN)

### 1. Setup Social Stream Ninja

1. Install Social Stream Ninja extension/app
2. Tambah platform: YouTube, TikTok, Twitch, atau Facebook
3. Buka SSN settings → **Mechanics**
4. Aktifkan dua checkbox:
   - ✅ Enable remote API control of extension
   - ✅ Send chat messages to API server
5. Restart SSN jika diminta

### 2. Ambil Session ID

1. Buka SSN dock di browser
2. Lihat URL: `https://socialstream.ninja/dock.html?session=[SESSION_ID]`
3. Copy `[SESSION_ID]`

### 3. Pasang ke OBS

**URL dengan SSN:**
```
http://localhost:8000/index.html?ssn=SESSION_ID_KAMU&op=KYU&call=APEX-09
```

**Properties:**
| Field | Nilai |
|-------|-------|
| URL | URL di atas |
| Width | 1920 |
| Height | 1080 |
| FPS | 30 |
| Shutdown when not visible | ✓ |
| Refresh when scene active | ✓ |

### 4. Verifikasi

Kirim chat di YouTube/TikTok/Twitch. Pesan seharusnya muncul dalam 1-2 detik di overlay.

---

## Struktur Data

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

**Badges yang dikenali:**
- `mod` → MOD (merah)
- `subscriber` → SUB (biru)
- `vip` → VIP (kuning)
- `verified` → VERIFIED (cyan)
- `isowner: true` → OWNER (emas)

**Platforms:**
- `youtube` → Username merah (#ff6b6b)
- `tiktok` → Username cyan (#00f0f0)
- `twitch` → Username ungu (#a970ff)
- Lainnya → Username cyan default

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

Edit file untuk ubah warna:

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

### Chat tidak muncul di OBS tapi muncul di browser

- Pastikan menggunakan `http://localhost` (bukan `file://`)
- Refresh Browser Source di OBS (klik refresh icon)
- Cek console browser (F12) untuk WebSocket errors
- Test dengan `?demo=1` dulu

### Demo mode chat tidak muncul

- Buka URL dengan `?demo=1`
- F12 → Console, lihat ada error atau tidak
- Refresh halaman

### Status STANDBY terus (live mode)

1. Samakan Session ID
2. Aktifkan dua API toggle di SSN settings
3. Restart SSN
4. Test pakai `?demo=1` dulu untuk verify overlay sehat

### Emoji tampil besar atau aneh

- Emoji unicode langsung pass-through, inherit font-size chat
- Jika masih besar, edit CSS `.msg .txt { font-size: ... }`

### Timezone salah

- Timezone auto-detect dari OS
- Override dengan `?tz=7` (untuk GMT+7 WIB)
- Format: `?tz=-5` untuk GMT-5, `?tz=0` untuk GMT

---

## Development

### File Structure

```
stream-police-overlay/
├── index.html          # Single-file overlay (HTML + CSS + JS)
├── README.md           # Dokumentasi
└── .git/               # Version control
```

### Tech Stack

- **HTML5** – Semantic markup
- **CSS3** – Animations, gradients, clip-path
- **Vanilla JS** – WebSocket, DOM rendering, timezone handling
- **Google Fonts** – Rajdhani, Share Tech Mono

### Modifikasi

Edit `HUD_CONFIG` object di `<script>` section untuk:
- Nama operator default
- Tactical status rows
- Badge styles
- Platform colors

---

## Tips Streaming

1. **Position di OBS:** Letakkan overlay di atas gameplay layer
2. **Size:** Default 1920x1080, scale down kalau perlu
3. **Alerts:** Chat panel auto-scroll ke message terbaru
4. **Performance:** Vanilla JS tanpa framework, CPU usage minimal
5. **Mobile viewers:** Layout responsive, cocok di berbagai res
6. **Multi-platform:** SSN merge YouTube + TikTok chat real-time

---

## License

Free to use & modify untuk GTA V RP streaming.

Untuk integrasi Social Stream Ninja, ikuti lisensi GPL-3.0 SSN:
- Discord: https://discord.socialstream.ninja
- GitHub: https://github.com/steveseguin/social_stream

---

## Support

**Issue atau saran?**

Buat issue di GitHub atau hubungi langsung.

**Kontribusi welcome** – fork, modifikasi, submit PR.

---

**Last updated:** Oktober 2026  
**Status:** Production Ready ✓
