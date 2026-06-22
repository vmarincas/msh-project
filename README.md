# Marvel Super Heroes — Tier List

Aplicație locală de tip *tier list* pentru setul Magic: The Gathering **Marvel Super Heroes** (cod set `MSH`).
Totul rulează în browser, fără server și fără build. Datele cardurilor vin de la [Scryfall](https://scryfall.com).

## Ce face
- Tier list cu rândurile S / A / B / C / D / F (drag & drop sau click pe card).
- Notă de la 1 la 10 pe fiecare card, afișată în colțul cardului.
- Sortare (implicit după numărul din set) + sortare după raritate, notă, nume, cost de mana.
- Filtre: căutare după nume, culoare, raritate.
- Salvare automată locală + salvarea imaginilor offline.
- Export/import: pachet `.zip` (aplicație + folderul `cards/` cu imagini + date) sau doar datele `.json`.

## Cum o pornești
Cel mai simplu — deschide `index.html` cu dublu-click în browser.

Sau, pentru un mic server local (recomandat ca să eviți orice limitare de `file://`):

```bash
# Python 3 (de obicei deja instalat)
python3 -m http.server 8000
# apoi deschide http://localhost:8000 în browser
```

La prima pornire (cu internet) descarcă lista de carduri de la Scryfall și o salvează local.
După aceea merge și offline.

## Cum o dezvolți cu Claude Code
1. Instalează Claude Code (vezi mai jos).
2. Intră în folderul proiectului și pornește:
   ```bash
   cd marvel-super-heroes-tierlist
   claude
   ```
3. Cere-i ce vrei, de exemplu: „adaugă un buton de export ca imagine PNG a tier list-ului".

### Instalare Claude Code
Necesită un plan plătit (Pro, Max, Team, Enterprise) sau cont Anthropic Console cu credite API.
Planul gratuit Claude.ai **nu** include Claude Code.

**Instalare nativă (recomandată — fără Node.js, se actualizează singură):**
```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash
```
```powershell
# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

**Alternativ, prin npm (necesită Node.js 18+):**
```bash
npm install -g @anthropic-ai/claude-code
```

Verifică instalarea: `claude --version` (sau `claude doctor` pentru diagnostic).
La prima rulare, `claude` te trece printr-un login în browser.

Documentație: https://code.claude.com/docs/en/setup

## Structura proiectului
```
marvel-super-heroes-tierlist/
├─ index.html     # toată aplicația (HTML + CSS + JS într-un singur fișier)
├─ README.md
├─ CLAUDE.md      # context pentru Claude Code
└─ .gitignore
```

## Note
Instrument neoficial, făcut de fani. Marvel Super Heroes™ și Magic: The Gathering™ aparțin
deținătorilor de drept (Marvel / Wizards of the Coast). Datele și imaginile provin de la Scryfall.
