# STF Tactical HUD - Police Streaming Overlay

OBS browser source overlay. Cyber police theme. San Andreas Law Enforcement (GTA V RP).

<p align="center">
  <img src="https://media.discordapp.net/attachments/1531751267859169573/1556376367736553704/image.png?backend=b2&ex=6ac3eff2&is=6ac29e72&hm=a3db8aa6236289ce42dc25fdf05c239ac49eaedada1ddcf890f3a1ae7b7aa046&=&format=webp&quality=lossless&width=1024&height=575" width="600" alt="STF Tactical HUD Preview">
</p>

## OBS Setup (offline)

1. OBS → Sources → `+` → Browser
2. Check `Local file`, pick `index.html`
3. Width `1920`, Height `1080`, FPS `30`
4. Append params to file path:

```text
file:///D:/stream-police-overlay/index.html?op=KYU&call=APEX-09&ssn=SESSION_ID&limit=30
```

Replace:
- `D:/stream-police-overlay/index.html` → your path
- `KYU` → operator name
- `APEX-09` → callsign
- `SESSION_ID` → SSN session
- `30` → max chat messages

Demo without SSN:

```text
file:///D:/stream-police-overlay/index.html?op=KYU&call=APEX-09&demo=1
```

## Params

| Param | Example | Use |
|---|---|---|
| `op` | `?op=KYU` | operator name |
| `call` | `?call=APEX-09` | callsign |
| `rank` | `?rank=SERGEANT` | rank |
| `badge` | `?badge=122` | badge number |
| `mission` | `?mission=TEXT` | mission text |
| `ssn` | `?ssn=abc123` | SSN session live chat |
| `demo` | `?demo=1` | fake chat test |
| `limit` | `?limit=30` | chat buffer size |
| `rec` | `?rec=0` | hide REC |

## SSN Session

1. Open SSN dock: `https://socialstream.ninja/dock.html?session=XXX`
2. Copy `XXX`
3. Use as `?ssn=XXX`
4. Enable in SSN Settings → Mechanics:
   - `Enable remote API control`
   - `Send chat messages to API`

Chat badge auto: MOD red, SUB blue, VIP yellow, OWNER gold. Username color by platform: YouTube red, TikTok cyan, Twitch purple.

## Fix

- No chat in OBS: use `http://localhost` via `python3 -m http.server`, not `file://` for WebSocket. Or test `?demo=1` first.
- Wrong time: auto local PC time.
- `STANDBY`: wrong session ID or SSN API off.
