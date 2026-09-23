# ledakanpemuda.com

Flat HTML, generated. Official site of Ledakan Pemuda — the album *Korupsi* (2025).

- `/` — **Poster Darah** (round 3, combo A): the horror one-sheet. Chosen 2026-09-23; Buku Darah (combo B) retired, draft kept at `drafts/round3/buku-darah/`.

## Source of truth
The pages are BUILD OUTPUT. Edit the generators, not these files:

- `/data/pat/websites/ledakanpemuda/drafts/round3/build_poster_darah.py`, `common.py`, `listen.py`
- then `./run.sh poster-darah` (needs `python3 -m http.server 8767` in that dir) and `python3 publish.py`
- facts: `/data/pat/websites/ledakanpemuda/PRODUCT.md`; lyrics: `assets/source/lyrics-stanzas.json` (Bandcamp, verbatim) + `lyrics-en.json` (working translation — review before trusting)

`publish.py` copies only the assets the pages link, rewrites paths root-absolute, writes the production head (canonical, og/twitter, JSON-LD MusicAlbum from the track list, favicon set), robots.txt, sitemap.xml, 404.html, CNAME.

## What changed for launch (round-3 critique P1s, fixed in `listen.py`)
- Play control tells the truth: BUKA PEMUTAR / OPEN THE BANDCAMP PLAYER → TUTUP PEMUTAR once open (closing removes the iframe; Bandcamp can't be started from outside its frame). One-line note with an exit to Bandcamp.
- Four outlets (Bandcamp, Spotify, Apple Music, YouTube Music) directly under the control; a fixed dock (Putar · outlets) once the hero scrolls away, off over IKUTI and the footer.
- Lyrics all closed by default; "[ Tutup lirik ]" at the foot of an open song returns focus to its title. Phone page ~7,300px (was ~9,500).

Analytics: none installed (add the host's own snippet to `head()` in `publish.py` when Patrick supplies it).
