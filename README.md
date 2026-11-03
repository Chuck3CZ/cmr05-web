# Colin McRae Rally 2005 in the browser

**English** · [Česky](#česky)

A fan project that runs the PC game *Colin McRae Rally 2005* (Czech edition, 2004) in Google Chrome, on a computer or
on a phone with touch controls. The game's x86 code is translated to C ahead of time and compiled to WebAssembly;
Win32, Direct3D 9, DirectInput 8 and DirectSound are reimplemented, and the graphics run on WebGPU. You bring your own
copy of the game.

Built on [wc3-web](https://github.com/Chuck3CZ/wc3-web) (same lifter and runtime, originally from
[nfsu2-web](https://github.com/Chuck3CZ/nfsu2-web), which is playable at [nfsu2.xyz](https://nfsu2.xyz)).

<!-- Live version: add the public address of your server here, e.g. **Play:** https://cmr.example.cz -->
<!-- Screenshot: add one to docs/img/ and show it here, e.g. ![A stage in Chrome](docs/img/stage.png) -->

> Not affiliated with or endorsed by Codemasters. This repository contains none of the game's code or data: no EXE,
> DLL, BIG or WMV, no C translated from them, no compiled module (`.gitignore` keeps them out).

## Status

| Part | State |
|---|---|
| Lifting `CMR5.EXE` and the `Math*.dll` (7,822 functions) and linking to WebAssembly | works |
| Direct3D 9 on WebGPU, menus, races, HUD | works |
| Sound: music, engine, co-driver | works |
| Keyboard, and touch controls for phones and tablets | works |
| Start page: browser check, loading ring, controls | works (`web/landing.html`) |
| Download the game data ahead (2.6 GB, without the videos), from the start page | works (`web/landing.html`, `src/host/predownload-worker.ts`) |
| Co-driver arrow in its box drawn too big; map at the start of a stage | known bugs |
| Videos (WMV), gamepad and wheel (DirectInput 8) | not yet |

The plan, the state and the measured numbers are in [docs/PLAN.md](docs/PLAN.md) (in Czech).

## What you need

- **The game:** *Colin McRae Rally 2005*, Czech edition. The exact files and their SHA-256 are in
  [docs/PLAN.md](docs/PLAN.md#vstupní-soubory-cz-vydání-2004); the data are 2.9 GB and the page streams them as you play.
- **To play:** a current Chrome or Edge with WebGPU, and a page served over HTTPS (or `localhost`). A keyboard, or a
  touch screen: the buttons over the game turn on by themselves on a touch device (`?touch` forces them, `?notouch`
  turns them off).
- **To build:** Linux or WSL 2, Python 3 with `pefile` and `capstone` 5, wasi-sdk in `deps/wasi-sdk`, Node.js 24.

## Run it

```sh
. tools/env.sh
./gen.sh "<CMR 2005 folder>"     # lift CMR5.EXE and the Math*.dll to C (gen/, never committed)
./build.sh                       # compile + link -> out/web/wc3.wasm
node web/build.mjs               # the page's scripts
node web/tools/wc3-dev-server.mjs --data "<CMR 2005 folder>"
```

Open the server's address (port 8086 by default). The start page checks the browser (secure page, threads, WebGPU,
storage), shows the module's version on the server, lets you pick a name, the picture's resolution and the touch
controls, shows a loading ring while the module downloads, and lists the controls. It can also download the game's
data (2.6 GB, without the videos the browser game does not play) ahead of play, so a stage never waits on the network:
the download can be stopped and continued, and starting a game stops it.

A QNAP / Docker Compose setup with a Cloudflare Tunnel is in [deploy/qnap/README.md](deploy/qnap/README.md) (in Czech).

## License

MIT ([LICENSE](LICENSE)). *Colin McRae Rally* is a trademark of its owner.

---

## Česky

Fanouškovský projekt, který spouští PC hru *Colin McRae Rally 2005* (české vydání z roku 2004) přímo v Google Chrome,
na počítači i na telefonu s dotykovým ovládáním. Kód hry (x86) se předem přeloží do C a zkompiluje do WebAssembly,
Win32, Direct3D 9, DirectInput 8 a DirectSound jsou napsané znovu a grafika běží na WebGPU. Vlastní kopii hry si
přineseš sám. Stojí na [wc3-web](https://github.com/Chuck3CZ/wc3-web).

**Stav:** menu, závody, HUD, zvuk (hudba, motor, spolujezdec), klávesnice a dotykové ovládání fungují. Známé chyby:
šipka spolujezdce v rámečku je zvětšená a mapa při startu etapy. Zatím chybí videa (WMV), gamepad a volant.
Podrobný plán a stav jsou v [docs/PLAN.md](docs/PLAN.md).

**Co potřebuješ:** české vydání hry (data mají 2,9 GB a stránka je stahuje průběžně), k hraní aktuální Chrome nebo
Edge s WebGPU a stránku přes HTTPS (nebo `localhost`). K sestavení Linux nebo WSL 2, Python 3 s `pefile` a
`capstone` 5, wasi-sdk v `deps/wasi-sdk` a Node.js 24.

**Spuštění:** příkazy jsou v části *Run it* výše. Nasazení na QNAP s Cloudflare Tunnelem je v
[deploy/qnap/README.md](deploy/qnap/README.md).

**Startovní stránka** (`web/landing.html`) před spuštěním zkontroluje prohlížeč, ukáže verzi modulu na serveru, nechá
zvolit jméno, rozlišení a dotykové ovládání, při stahování modulu ukazuje načítací kroužek a obsahuje přehled ovládání.
Umí také stáhnout data hry (2,6 GB, bez videí) předem, takže trať nečeká na síť. Stahování jde zastavit a později
pokračovat, spuštění hry ho samo zastaví.
Je to jediný soubor bez závislostí, protože server posílá COEP `require-corp`.

> Projekt nijak nesouvisí s Codemasters a není jimi schválen. Repozitář neobsahuje žádný kód ani data hry.
