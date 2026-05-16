# Taiwan — 15 giorni

> Landing page editoriale a scroll verticale per un viaggio di 15 giorni a Taiwan (6 → 20 luglio 2026).

Lo scroll dall'alto in basso *è* il viaggio: si parte da Taipei al nord, si scende lungo la costa ovest passando per Lukang, Meinong e Tainan, si attraversa il sud, si risale la costa est del Pacifico tra Dulan e Chishang, e si rientra a Taipei in treno. La mappa di Taiwan a destra disegna il percorso man mano che leggi.

## Deploy

Il sito è ospitato su **Netlify** come sito statico — nessun build step, nessun framework, nessuna dipendenza da installare.

### Deploy iniziale

1. Carica il repo su GitHub.
2. Da [app.netlify.com](https://app.netlify.com) → **Add new site** → **Import an existing project** → seleziona il repo.
3. Lascia i campi di build vuoti (publish directory: `.`). Netlify userà automaticamente `netlify.toml`.
4. **Deploy site**. Ogni push su `main` ri-deploya.

Alternativa zero-config: `netlify deploy --prod` dalla root del repo dopo `npm i -g netlify-cli`. Oppure trascina la cartella su [netlify.com/drop](https://app.netlify.com/drop).

### File di configurazione

- `netlify.toml` — publish dir, cache headers, security headers basilari.
- `robots.txt` — apertura standard ai crawler.
- `index.html` — copia di `Taiwan 15 giorni.html`, è la pagina servita di default.

## Demo locale

Apri `index.html` o `Taiwan 15 giorni.html` in qualsiasi browser moderno. Nessun build step. Funziona anche aperto da `file://`.

## Versione standalone offline

Per condividere il sito via mail / WhatsApp / AirDrop senza passare dal deploy:

- `Taiwan 15 giorni (standalone).html` — ~8.7 MB, font e SVG inlinati. Un solo file, niente network richiesto.

Questo file **non serve** per il deploy Netlify (sarebbe uno spreco di banda: meglio servire i font dal CDN di Google). Usalo solo per la condivisione diretta o per l'archivio.

## Struttura repo

| File | Cosa contiene |
| --- | --- |
| `index.html` | Pagina principale servita da Netlify. |
| `Taiwan 15 giorni.html` | Copia identica con nome leggibile (per editing manuale). |
| `Taiwan 15 giorni (standalone).html` | Variante standalone offline, ~8.7 MB. |
| `Taiwan 15 giorni (standalone source).html` | Sorgente con thumbnail-splash per il bundler. |
| `netlify.toml` | Configurazione Netlify. |
| `robots.txt` | Indicizzazione aperta. |
| `uploads/itinerario-taiwan-luglio-2026.md` | Itinerario sorgente in Markdown. |

> Nota: `index.html` e `Taiwan 15 giorni.html` sono lo stesso contenuto. Se modifichi una versione, ricordati di sincronizzare l'altra (o rimuovi quella che preferisci e usa solo `index.html`).

## Concept narrativo

- **Scroll = viaggio geografico**: top → bottom corrisponde all'asse nord → sud → est → nord di nuovo.
- **Mappa scroll-linked**: il percorso si disegna progressivamente con `stroke-dashoffset`. Linee continue per l'auto, tratteggiate per HSR e Puyuma, punto isolato per Fo Guang Shan.
- **Nav giorni sticky**: 15 puntini a sinistra, sempre visibili. Click per saltare al giorno, highlight automatico del giorno corrente via `IntersectionObserver`.
- **Sezioni di transito** dedicate tra una tappa e l'altra, con mezzo e durata.

## Design system

### Palette

| Token | Valore | Uso |
| --- | --- | --- |
| `--bg` | `#F5F1EA` | Sfondo sabbia chiaro |
| `--ink` | `#1A1A1A` | Testo principale |
| `--ink-mute` | `#7A746B` | Metadati, etichette mono |
| `--rule` | `#D9D2C3` | Separatori |
| `--tea` | `#5B6E4A` | Verde foglia di tè — Taiwan rurale, Meinong, Chishang |
| `--brick` | `#8B4A2B` | Terra di Siena — templi, Lukang, Tainan, Taipei |
| `--pacific` | `#355A6B` | Blu Pacifico — costa est, Dulan |

### Typography

- **Cormorant Garamond** — display, titoli, numeri dei giorni, citazioni in corsivo
- **IBM Plex Sans** — body 300/400/500
- **IBM Plex Mono** — etichette, orari, eyebrow, monospace tabulari
- **Noto Serif TC** — nomi di luoghi e hotel in cinese tradizionale

### Riferimenti

Editorial design giapponese, *Cereal Magazine* / *Kinfolk* / *Apartamento*, estetica Dadaocheng contemporanea, wabi-sabi nei dettagli (grain, asimmetrie controllate, generoso whitespace).

## Personalizzazione

Tutto il codice è in un singolo file HTML, leggibile dall'alto verso il basso:

- `:root { --bg, --ink, --tea, --brick, --pacific }` per la palette — cambia un token, si propaga ovunque.
- Le sezioni-giorno sono `<section class="day-section" data-day="N" data-accent="tea|brick|pacific">`. Aggiungi/togli sezioni e aggiorna la lista `DAYS` nel `<script>` in fondo per ricalibrare la mappa.
- I segmenti del percorso sulla mappa SVG sono `<path id="seg-N">`. La mappa `SEG_FOR_DAY` nel JavaScript dice quali segmenti sono disegnati a quale giorno.
- `ACTIVE_CITY` e `CITY_FOR_DAY` controllano quale punto è evidenziato e quali sono già stati "visitati".

## Tecnica

- Single file, HTML + CSS + JS inline. Nessun framework.
- SVG inline per la mappa, niente immagini bitmap pesanti.
- Animazioni gestite con `IntersectionObserver` + transizioni CSS.
- Mobile-first responsive: due breakpoint (`980px`, `640px`). Su mobile la mappa diventa una striscia sticky in alto, le sezioni si impilano.
- Print stylesheet incluso: ogni giorno su pagina propria con `page-break-inside: avoid`.
- Accessibilità: contrasti AA, navigazione da tastiera, focus state.

## Itinerario in breve

| Giorni | Tappa | Hotel |
| --- | --- | --- |
| 1–3 (6–8 lug) | Taipei — Zhongshan | Humble Boutique Hotel |
| 4–5 (9–10 lug) | Lukang — 鹿港 | Yong Le, Tribute Portfolio |
| 6 (11 lug) | Meinong — 美濃 | Yellow & Black Guest House |
| 7–8 (12–13 lug) | Tainan — 臺南 | Hotel Initial-Tainan |
| 9–10 (14–15 lug) | Dulan — 都蘭 | La Tree Homestay |
| 11–12 (16–17 lug) | Chishang — 池上 | How Has Homestay |
| 13–15 (18–20 lug) | Taipei — Dadaocheng | Tomato TTT |

Logistica auto: ritiro Taichung HSR il 9 luglio, consegna Taitung TRA station il 18 luglio. Rientro a Taipei in Puyuma Express 417.

## Licenza

Progetto personale. L'itinerario, le note di viaggio e i contenuti editoriali sono privati: se forki il repo per il tuo viaggio, sostituisci i contenuti con i tuoi.
