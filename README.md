# 🍓 RPi Homelab Configurator

Interaktywny konfigurator serwisów dla Raspberry Pi.  
Dwa pliki — `index.html` + `services.json` — zero dependencji, działa bez serwera.

## Uruchomienie

```bash
git clone https://github.com/twoj-login/rpi-homelab-configurator
cd rpi-homelab-configurator
```

**Opcja A — przez prosty serwer HTTP** (zalecane, ładuje `services.json` automatycznie):
```bash
python3 -m http.server 8080
# otwórz http://localhost:8080
```

**Opcja B — bezpośrednio z dysku** (`file://`):
```bash
xdg-open index.html   # Linux
open index.html        # macOS
start index.html       # Windows
```
> Przy otwieraniu przez `file://` przeglądarka może zablokować `fetch()` do `services.json`.
> Wtedy załaduj plik ręcznie przez zakładkę **Serwisy → Import/Export → przeciągnij services.json**.

## Funkcje

### ⚙ Konfigurator
- Wybierz model RPi (3 / 4 / 5 / dowolny) — serwisy wymagające mocniejszego sprzętu są wyszarzone
- Kliknij karty serwisów, żeby je zaznaczyć
- Pasek RAM w nagłówku zmienia kolor gdy przekraczasz limit modelu
- Na każdej karcie: **badge licencji** (klikalny → URL), **ikona komercyjnego użycia**, **link do repo**
- Kliknij **Generuj pliki** → pobierz `docker-compose.yml`, `.env.example`, `README.md`

### 📦 Serwisy (panel zarządzania)
- **Dodaj własny serwis** — formularz z pełnymi polami (licencja, repo, compose snippet)
- **Edytuj istniejący** — przycisk ✏ przy każdym wierszu
- **Usuń** — przycisk ✕ (własne i domyślne)
- **Eksportuj services.json** — pobierz zaktualizowany plik, podmień w repo
- **Importuj services.json** — przeciągnij plik lub kliknij drop-zone
- **Przywróć domyślne** — reset do wbudowanej listy

## Struktura pliku services.json

```json
[
  {
    "id": "my-service",          // unikalny klucz (bez spacji)
    "cat": "Kategoria",          // kategoria (istniejąca lub nowa)
    "emoji": "🐳",
    "name": "Mój Serwis",
    "desc": "Krótki opis",
    "ram": 256,                  // MB
    "disk": 2,                   // GB
    "port": "3000",
    "minRpi": 3,                 // 3, 4 lub 5
    "license": "MIT",
    "licenseUrl": "https://github.com/.../LICENSE",
    "repoUrl": "https://github.com/...",
    "commercial": true,          // true = dozwolone, false = niedozwolone
    "compose": "  my-service:\n    image: myimage:latest\n    ..."
  }
]
```

## Workflow dodawania nowego serwisu

1. Otwórz konfigurator → zakładka **Serwisy**
2. Wypełnij formularz i kliknij **Zapisz serwis**
3. Kliknij **Eksportuj services.json**
4. Skopiuj pobrany plik do folderu repo (nadpisz stary)
5. Zrób `git add services.json && git commit -m "Add: NazwaSerwisu"`

## Kolory licencji

| Badge | Licencje | Komercyjne |
|-------|----------|------------|
| 🟢 zielony | MIT, Apache, ISC, BSD, EPL | zwykle ✅ |
| 🟠 pomarańczowy | GPL, AGPL, EUPL | zależy od użycia |
| 🔴 czerwony | Proprietary, Sustainable Use, SSPL | ograniczone |

## Struktura repo

```
rpi-homelab-configurator/
├── index.html       # UI (konfigurator + panel zarządzania)
└── services.json    # dane serwisów (edytowalne)
```
