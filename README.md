# FaunaTor

Tor inside a web page. Read ordinary web pages and v3 `.onion` services from any browser, with no extension, no install and no server of your own.

**Online: https://mauktenieb.github.io/faunator/**

## How it works

Two Tor clients compiled to WebAssembly run in the page and reach the Tor network through Snowflake, the Tor Project's own bridges (WebSocket / WebRTC):

| File | Source | Reaches |
|---|---|---|
| `tor-web.js`, `tor-web_bg.wasm` | [privacy-ethereum/webtor-rs](https://github.com/privacy-ethereum/webtor-rs) v0.5.7 (MIT) | any http(s) page, through a Tor exit |
| `tor-onion.js`, `tor-onion_bg.wasm` | [andrewtheguy/webtor-rs](https://github.com/andrewtheguy/webtor-rs) v0.0.1-alpha.25 (MIT) | v3 onion services, `http://` and `ws://` |

`faunator-tor.js` loads them and keeps the onion client's directory (a few MB, valid about three hours) in IndexedDB, so the second visit starts faster.

For ordinary pages, FaunaTor also races public CORS relays and keeps the first good answer; your own relay goes first when set (see below). Pages are shown with their scripts removed, and links and forms stay inside FaunaTor.

## Files

- `index.html`: the browser (address bar and page view).
- `faunator-tor.js`: loads the two Tor clients.
- `faunator-worker.js`: an optional relay of your own, as a free Cloudflare Worker (100,000 requests a day). It only answers the origins you allow. Paste it in dash.cloudflare.com → Workers & Pages → Create → Worker, then set `RELAY` in `index.html`.
- `.github/workflows/build.yml`: rebuilds the two clients from their sources (Rust, wasm-pack) and commits them.
- `.github/workflows/consensus.yml`: fetches the Tor consensus and microdescriptors twice a day and publishes them under `tor/`, so the web client reads them over GitHub Pages. The live site reads the copy that Insert Koin refreshes (https://mauktenieb.github.io/insertkoin/tor/), so this workflow only runs by hand here. Set `CONSENSUS_BASE` in `build.yml` to your own Pages URL + `/tor` if you fork.

## Limits

- Snowflake and in-page Tor are slower than Tor Browser: count tens of seconds for the first circuit.
- This is not Tor Browser: no fingerprinting protection, and the page around it is an ordinary web page. Use Tor Browser when anonymity matters.
- A `.onion` site that needs JavaScript will not run: scripts are removed.

<!-- network:start -->
## Mauk Tenieb on GitHub

- [mauktenieb.github.io](https://mauktenieb.github.io/): Start here: every site and link.
- [katabase](https://github.com/MaukTenieb/katabase) · [site](https://mauktenieb.github.io/katabase): Korhogo Fauna: concept albums, 18 avatars, lore.
- [insertkoin](https://github.com/MaukTenieb/insertkoin) · [site](https://mauktenieb.github.io/insertkoin/): Insert Koin, the browser arcade: Puck You!, Fauna Chess, Erratik, Faunarratik, Katabatik.
- [3615](https://github.com/MaukTenieb/3615) · [site](https://mauktenieb.github.io/3615/): 3615 KORHOGO, a Minitel terminal in the browser.
- [korhogo](https://github.com/MaukTenieb/korhogo) · [site](https://mauktenieb.github.io/korhogo/): The first Korhogo site.
- [reporhogo](https://github.com/MaukTenieb/reporhogo) · [site](https://mauktenieb.github.io/reporhogo/): One topic, forty-one code forges (MIT).
- [Photofauna](https://github.com/MaukTenieb/Photofauna) · [site](https://mauktenieb.github.io/insertkoin/fotofauna.html): The pocket photo emulator.
- [detour](https://github.com/MaukTenieb/detour): Cuts a sheet of eight portraits into a sprite strip (Python).
- [torview](https://github.com/MaukTenieb/torview) · [site](https://mauktenieb.github.io/torview/): Minimal Tor webview browser (Go).
- [korhotube](https://github.com/MaukTenieb/korhotube): All the videos of a YouTube channel as JSON: tags, chapters, dubbing, dates (MIT).

Artist: https://github.com/MaukTenieb · Music and sales: https://mauktenieb.bandcamp.com · Journal: https://korhogo.substack.com · Bio: https://orcid.org/0009-0007-9096-8267
<!-- network:end -->

## License

Code: MIT (see `LICENSE`). Tor clients: MIT, see `TOR-LICENSES.txt`. The names FaunaTor, Korhogo and Korhogo Fauna are not covered by the license.

By [Mauk Tenieb](https://mauktenieb.github.io/). Also part of [Insert Koin](https://mauktenieb.github.io/insertkoin/).
