# CLAUDE.md — context pentru Claude Code

## Ce e proiectul
Aplicație *tier list* pentru setul MTG **Marvel Super Heroes** (cod `MSH`).
Un singur fișier: `index.html` conține HTML + CSS + tot JavaScript-ul (vanilla, fără framework, fără build).
Rulează în browser, fără server. Se poate deschide direct (`file://`) sau servit static.

## Arhitectură (totul în `index.html`)
- **CSS**: în `<style>`. Temă „comic book" pe variabile CSS în `:root` (`--ink`, `--pop`, `--kapow`, culori de tier `--s..--f`). Fonturi: Anton (display) + Inter (body) de la Google Fonts, cu fallback de sistem.
- **JS**: într-un singur `<script id="appcode">`, IIFE `"use strict"`. Fără dependențe la runtime, în afară de **JSZip** încărcat lazy de la CDN doar la export/import `.zip`.

## Modelul de date
`state` (salvat în `localStorage`, cheia `msh-tierlist-v2`):
- `cards`: `{ id: {id,name,img,colors,rarity,type,cmc,num} }` — catalog, populat de la Scryfall.
- `placements`: `{ cardId: tierId }` — în ce tier (S/A/B/C/D/F) e cardul; absent = „Neclasate".
- `grades`: `{ cardId: 1..10 }` — nota cardului (badge în colț).
- `tiers`: listă de `{id,label}`.
- `ui` (doar în memorie): `size`, `colors`, `rarity`, `q`, `sort`, `dir`.

Imaginile cardurilor se cache-uiesc ca Blob în **IndexedDB** (`msh-tl-img` / store `images`, cheie = id card).
La boot se reconstruiesc object URL-uri din ele (`imgURL`, `imgBlobs`).

## Surse externe
- Lista de carduri: Scryfall search API
  `https://api.scryfall.com/cards/search?q=set:msh -t:basic -is:digital -t:token&unique=cards&order=set` (paginat).
- Imaginile: URL-uri Scryfall din `image_uris.normal` (sau `card_faces[0].image_uris.normal` la cardurile cu două fețe).

## „Packaged mode" (export pachet)
`Exportă pachet` produce un `.zip` cu:
- `index.html` regenerat (din `document.documentElement.outerHTML`) în care se injectează
  `window.__PACKAGED_DATA__ = {state, imgBase:"cards/"}` ÎNAINTE de scriptul `#appcode`;
- folderul `cards/<id>.jpg` cu imaginile;
- `tierlist.json` cu datele.
La boot, dacă există `window.__PACKAGED_DATA__`, aplicația încarcă din el și folosește imagini locale
(`IMG_BASE = "cards/"`), fără internet. Funcțiile online (Scryfall, „Salvează offline") sunt ascunse.

## Convenții
- Vanilla JS, ES5-friendly (fără template literals în zonele care ajung în pachet, ca să nu strice heredoc-uri/escape). Păstrează stilul.
- Tot ce scrie pe disc/rețea e în `try/catch`; degradează grațios (preview fără `localStorage`, offline fără Scryfall).
- Când injectezi date într-un `<script>`, escapează `<` (`replace(/</g,"\\u003c")`).
- UI-ul e în română. Păstrează limba.

## Gotchas
- `localStorage`/`IndexedDB` pot fi blocate în sandbox-uri (preview) — codul tratează asta.
- „Salvează offline" și pachetul `.zip` au nevoie ca Scryfall să permită `fetch` cross-origin (CORS) ca să obțină Blob-urile; afișarea prin `<img>` merge oricum.
- JSZip se ia de la `cdnjs.cloudflare.com` — la export `.zip` e nevoie de internet o dată.

## Idei de continuat
- Export ca imagine PNG a întregului tier list (html2canvas sau desen pe `<canvas>`).
- Note text per card, nu doar numerice.
- Tier-uri configurabile (adăugare/ștergere/reordonare rânduri).
- Comutator între prints/variante de artă.
