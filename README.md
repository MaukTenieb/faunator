# FaunaTor

FaunaTor runs two Tor clients inside a web page and reads ordinary pages and v3 `.onion` services from any browser.

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
## Provenance

Built for [Insert Koin](https://mauktenieb.github.io/insertkoin/), Mauk Tenieb's browser games set in Korhogo: it runs its FaunaTor room.

Other tools by Mauk Tenieb: [Detour](https://github.com/MaukTenieb/detour): cuts portrait sheets into sprite strips and places the eye lights · [KorhoTube](https://github.com/MaukTenieb/korhotube): lists every video of a YouTube channel as JSON · [Reporhogo](https://github.com/MaukTenieb/reporhogo): searches forty-one code forges for a topic and lists the repositories it finds · [Photofauna](https://github.com/MaukTenieb/Photofauna): a photo editor with 44 looks, part of Insert Koin · [TorView](https://github.com/MaukTenieb/torview): a desktop browser in Go that routes its traffic through the official Tor daemon.

Index of Mauk Tenieb's sites: https://mauktenieb.github.io/
<!-- network:end -->

## License

Code: MIT (see `LICENSE`). Tor clients: MIT, see `TOR-LICENSES.txt`. The names FaunaTor, Korhogo and Korhogo Fauna are not covered by the license.

By [Mauk Tenieb](https://mauktenieb.github.io/). Also part of [Insert Koin](https://mauktenieb.github.io/insertkoin/).
