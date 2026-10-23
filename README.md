# FaunaTor

By Mauk Tenieb & Korhogo. Build guide: [MaukTenieb/method](https://github.com/MaukTenieb/method).

FaunaTor runs two Tor clients inside a web page and reads ordinary pages and v3 `.onion` services from any browser.

Online: https://mauktenieb.github.io/faunator/

## How it works

Two Tor clients compiled to WebAssembly run in the page. They reach the Tor network through Snowflake, the Tor Project's own bridges (WebSocket / WebRTC).

| File | Source | Reaches |
|---|---|---|
| `tor-web.js`, `tor-web_bg.wasm` | [privacy-ethereum/webtor-rs](https://github.com/privacy-ethereum/webtor-rs) v0.5.7 (MIT) | any http(s) page, through a Tor exit |
| `tor-onion.js`, `tor-onion_bg.wasm` | [andrewtheguy/webtor-rs](https://github.com/andrewtheguy/webtor-rs) v0.0.1-alpha.25 (MIT) | v3 onion services, `http://` and `ws://` |

`faunator-tor.js` loads them and keeps the onion client's directory (a few MB, valid about three hours) in IndexedDB, so the second visit starts faster.

For ordinary pages, FaunaTor also races public CORS relays and keeps the first good answer. Your own relay goes first when you set one (see below). FaunaTor removes the scripts from each page it shows and keeps links and forms inside FaunaTor.

## Files

- `index.html`: the browser (address bar and page view).
- `faunator-tor.js`: loads the two Tor clients.
- `faunator-worker.js`: an optional relay of your own, as a free Cloudflare Worker (100,000 requests a day). It answers only the origins you allow. Paste it in dash.cloudflare.com → Workers & Pages → Create → Worker, then set `RELAY` in `index.html`.
- `.github/workflows/build.yml`: rebuilds the two clients from their sources (Rust, wasm-pack) and commits them.
- `.github/workflows/consensus.yml`: fetches the Tor consensus and microdescriptors twice a day and publishes them under `tor/`, so the web client reads them over GitHub Pages. The live site reads the copy that Insert Koin refreshes (https://mauktenieb.github.io/insertkoin/tor/), so here this workflow runs by hand. On a fork, set `CONSENSUS_BASE` in `build.yml` to your own Pages URL + `/tor`.
- `TOR-LICENSES.txt`: the licences of the two Tor clients.

## Limits

- Snowflake and in-page Tor run slower than Tor Browser: count tens of seconds for the first circuit.
- FaunaTor is an ordinary web page around two Tor clients; fingerprinting protection comes with Tor Browser, so use Tor Browser when anonymity matters.
- FaunaTor removes scripts, so a `.onion` site that needs JavaScript stays inert.

<!-- network:start -->
## Provenance

Built for [Insert Koin](https://mauktenieb.github.io/insertkoin/), Mauk Tenieb's Korhogo world, games and machines in a browser: it runs its FaunaTor room.

Other tools by Mauk Tenieb: [Detour](https://github.com/MaukTenieb/detour): cuts portrait sheets into sprite strips and places the eye lights · [KorhoTube](https://github.com/MaukTenieb/korhotube): lists the public videos of a YouTube channel as JSON · [Reporhogo](https://github.com/MaukTenieb/reporhogo): searches forty-one code forges for a topic and lists the repositories it finds · [Photofauna](https://github.com/MaukTenieb/Photofauna): a photo editor with 44 looks, part of Insert Koin · [TorView](https://github.com/MaukTenieb/torview): a desktop browser in Go that routes its traffic through the official Tor daemon.

Index of Mauk Tenieb's sites: https://mauktenieb.github.io/
<!-- network:end -->

Related: [mauktenieb.github.io](https://mauktenieb.github.io/) · [MaukTenieb/method](https://github.com/MaukTenieb/method) · [Insert Koin](https://mauktenieb.github.io/insertkoin/)

## Rights

Code under MIT, method under CC BY 4.0, Korhogo material reserved; credit Mauk Tenieb & Korhogo. Details: [LICENSE](LICENSE). The two Tor clients come under MIT from their authors: see `TOR-LICENSES.txt`.
