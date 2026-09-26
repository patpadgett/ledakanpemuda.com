# ledakanpemuda.com

Flat HTML, generated. Official site of Ledakan Pemuda — the album *Korupsi* (2025).

- `/` — **Poster Darah** (round 3, combo A): the horror one-sheet. Chosen 2026-09-23; Buku Darah (combo B) retired, draft kept at `drafts/round3/buku-darah/`.

## Source of truth
The pages are BUILD OUTPUT. Edit the generators, not these files:

- `/data/pat/websites/ledakanpemuda/drafts/round3/build_poster_darah.py`, `common.py`, `listen.py`
- then `./run.sh poster-darah` (needs `python3 -m http.server 8767` in that dir) and `python3 publish.py`
- facts: `/data/pat/websites/ledakanpemuda/PRODUCT.md`; lyrics: `assets/source/lyrics-stanzas.json` (Bandcamp, verbatim) + `lyrics-en.json` (working translation — review before trusting)

`publish.py` copies only the assets the pages link, rewrites paths root-absolute, writes the production head (canonical, og/twitter, JSON-LD MusicAlbum from the track list, favicon set, inlined font CSS + preloads), robots.txt, sitemap.xml, 404.html, CNAME.

## Performance pass (2026-09-24)
Lighthouse 67 → 84, LCP 15.8 s → 4.5 s (throttled mobile), CLS 0.101 → 0, phone first view 2.8 MB → 0.7 MB. Images are sized for their slots in WebP/AVIF with CSS filters baked in (`derive_images.py`); fonts are subset with metric-matched local fallbacks (`fonts.py`); `fonts.css` is gone (inlined). Details and before/after reports: `drafts/round3/README.md` § Performance pass, `drafts/round3/lh/`.

## Harden passes (2026-09-24 → 25, published 2026-09-25)
Three rounds against the built page (`harden_probe.cjs`, `harden2_probe.cjs`, `harden3_probe.cjs`, plus `pub_harden.cjs` / `pub_engines.cjs` on this tree). What the live site now does that it did not:
- Player states tell the truth: "Memuat pemutar Bandcamp… / Loading" with a spinner until the frame's `load`; "Tekan ▸ di pemutar" once it lands; boxed notes for blocked / offline / slow embeds with the Bandcamp exit in both languages; closing says it stops playback; double taps are one action; the dock control reads "Ke pemutar" while the player is open (it scrolls back, never closes).
- No JavaScript: the play control is a real link to the album on Bandcamp; JS-only controls stay hidden until the script runs. Each behaviour ships as its own `<script>`, so one failing block cannot take the others down.
- "Buka semua lirik" re-syncs on every `toggle` (open all → close one → it reads "Buka semua" again and its next click opens the rest). `[ Tutup lirik ]` returns focus to the song's title. Deep links (`#lirik-05`) open the song with its title in view.
- Landmarks: `<main>` around LAGU / CERITA / IKUTI; 44 px skip link. Focus rings on all 30 Tab stops, tuned per ground (blood on paper 5.4:1, ink on yellow 12.6:1, cream on the dock 17:1).
- Lettering plates fall back to Anton in the same colour if a mask image fails; forced-colors keeps plate ink and gives every control a 2 px border; `prefers-contrast: more` gets a near-solid plate behind hero text and steps muted browns to ink; reduced motion stops the spinner and the pocong drift.
- Reflow: no clipped or spilled text at 280 / 320 / 390 / 720 (200 % zoom) / 360 (400 %) / 1440, including German-length labels, a 60-character title, CJK and RTL lyric lines. Print: white sheet, plates set in type, controls and decoration off.
- 404: requested path shown as text (never HTML), percent-decoded when valid, cut at 120 characters, wraps anywhere at 320 px; `min-height: 100vh` before `100svh` for old engines.
- Verified in Chromium, WebKit and Firefox (Playwright). Untested: real Windows High Contrast (emulation only), a physical device's `navigator.onLine`, screen-reader announcement of the `role=status` notes.

## Polish pass (2026-09-26)
Final pass on the rendered page at 320-1440 (`polish_evidence.cjs`, measurements + screenshots read by a vision model), then one fix batch and one confirm round.
- IKUTI outlets: an even grid of equal blocks (3x2 wide, 2x3 from 1180 down and on phones at 16 px Anton, one stack under 375) instead of a ragged wrap (4/2, 3/2/1, 2/1/1/1/1). Every label single-line; Instagram/Facebook share the columns.
- Section plates stand on their ink, not the PNG's clear space: LAGU's top ink meets the list's first rule, CERITA's meets the photo top (negative top margins = the plate's measured alpha inset). Text fallback and print undo them.
- Billing block on tablet portrait (561-806 px): a 2x2 grid with real columns (the centred flex fell 3+1, then 2+2 with nothing aligned).
- Footer text stands on the 1240 px content column's edge (it sat at the full-bleed 52 px inset, 100 px left of everything above it).
- Story measure 32.5em (~70 characters; 34.1em ran to 76); `text-wrap:pretty` on story paragraphs and the blood lines; the closing quote of a blood line is bound to its last word (a 13-letter word made a one-word last line at three widths). No single-word last lines remain at 320-1440.
- Durations in tabular figures (colons stack); the toggle's larger +/- sign pulled onto the caps' optical centre; hover styles behind `(hover:hover)` so a tap no longer leaves the drop marker / underline stuck on a touch screen; scrollbar coloured from the palette.
- Dock on small phones: outlets never shrink (`flex:none`); the label is 15 px from 389 down and screen-reader-only under 364; a too-long translated label ellipsizes instead of pushing an outlet off screen.
Re-verified after the batch: harden3 probe (30 Tab stops ringed, zoom 200/400 %, long/CJK/RTL text, no-JS, print), published-tree probe (404 hardening, JSON-LD, states at three widths), WebKit + Firefox states, detector (22 warnings, all the poster-form set from the critique, none new). Deliberately left: the 1440 story column runs to ~585 px on a 1240 grid (right column is the photo), and the third empty cell in the social row of the IKUTI grid.

## What changed for launch (round-3 critique P1s, fixed in `listen.py`)
- Play control tells the truth: BUKA PEMUTAR / OPEN THE BANDCAMP PLAYER → TUTUP PEMUTAR once open (closing removes the iframe; Bandcamp can't be started from outside its frame). One-line note with an exit to Bandcamp.
- Four outlets (Bandcamp, Spotify, Apple Music, YouTube Music) directly under the control; a fixed dock (Putar · outlets) once the hero scrolls away, off over IKUTI and the footer.
- Lyrics all closed by default; "[ Tutup lirik ]" at the foot of an open song returns focus to its title. Phone page now ~5,600px (was ~9,500 at launch; the distill pass of 2026-09-24 cut everything the page said twice).

Analytics: none installed (add the host's own snippet to `head()` in `publish.py` when Patrick supplies it).
