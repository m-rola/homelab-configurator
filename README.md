# 🍓 RPi Homelab Configurator

An interactive configurator for self-hosted Raspberry Pi services.  
Pick what you want to run, check RAM requirements, and generate ready-to-use `docker-compose.yml` files — no build step, no dependencies, works offline after the first load.

**Live demo:** [your-login.github.io/homelab-configurator](https://your-login.github.io/homelab-configurator)

---

## Features

**Configurator tab**
- Filter services by RPi model (3 / 4 / 5 / any) — incompatible services are greyed out automatically
- RAM usage bar in the header changes colour as you approach the model's limit
- Each service card shows: **license badge** (clickable → license URL), **commercial use indicator**, **repo link**
- Click *Generate files* to download `docker-compose.yml`, `.env.example` and `README.md`

**Services tab**
- Add, edit or delete services via a form — no JSON editing required
- All changes are saved to browser `localStorage` automatically
- When you update `services.json` in the repo and reload, a merge banner appears offering to pull in new services without overwriting your custom ones
- Export the full list back to `services.json` to commit to the repo

---

## Quick start

```bash
git clone https://github.com/your-login/homelab-configurator
cd homelab-configurator
python3 -m http.server 8080
# open http://localhost:8080
```

Or open `index.html` directly — if the browser blocks `fetch()` on `file://`, import `services.json` manually via the Import panel in the Services tab.

---

## How persistence works

| Scenario | Behaviour |
|---|---|
| First open (HTTP server) | Fetches `services.json`, saves to localStorage |
| Every subsequent open | Loads from localStorage instantly — works offline |
| New services added to repo | Banner appears: *Merge new* or *Replace all* |
| Add/edit/delete via UI | Saved to localStorage immediately |
| Export → commit to repo | localStorage becomes the source of truth in the repo |
| Clear cache button | Wipes localStorage, re-fetches from file on next load |

---

## services.json schema

```json
[
  {
    "id": "my-service",         // unique key (no spaces)
    "cat": "Category",          // existing or new category
    "emoji": "🐳",
    "name": "My Service",
    "desc": "Short description",
    "ram": 256,                 // MB
    "disk": 2,                  // GB
    "port": "3000",
    "minRpi": 3,                // 3, 4 or 5
    "license": "MIT",
    "licenseUrl": "https://github.com/.../LICENSE",
    "repoUrl": "https://github.com/...",
    "commercial": true,         // true = allowed, false = not allowed
    "compose": "  my-service:\n    image: myimage:latest\n    ..."
  }
]
```

---

## Adding a new service — workflow

1. Open the configurator → **Services** tab
2. Fill in the form and click **Save service**
3. Click **Export services.json** and download the file
4. Replace `services.json` in the repo with the downloaded file
5. Commit: `git add services.json && git commit -m "feat: add MyService"`

---

## License colour coding

| Badge colour | Licenses | Commercial use |
|---|---|---|
| 🟢 Green | MIT, Apache, ISC, BSD, EPL, Zlib | ✅ typically allowed |
| 🟠 Orange | GPL, AGPL, EUPL | ⚠ depends on use case |
| 🔴 Red | Proprietary, Sustainable Use, SSPL | ⛔ restricted |

---

## Tech stack

Pure HTML + CSS + Vanilla JS — no framework, no build step, no CDN dependencies.  
Data lives in `services.json`. State persists in `localStorage`.

---

## Repository structure

```
homelab-configurator/
├── index.html       # full UI (configurator + service manager)
├── services.json    # service definitions (editable)
├── .nojekyll        # disables Jekyll on GitHub Pages
└── README.md
```
