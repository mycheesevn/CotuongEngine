# CotuongEngine — Pikafish corresponding source (GPLv3)

This repository is a **mirror of the Pikafish xiangqi engine source code**,
kept as the GPLv3 "corresponding source" home for the engine build evaluated
for the SynerWorlds Xiangqi web game (PacClone, cotuong.synerworlds.com).

- **Upstream:** https://github.com/official-pikafish/Pikafish
- **Mirrored commit:** `9d7833aa113626b3ea63ad83f49fb9b456bd5839`
  (upstream `master`, 2026-10-25, identifies itself as `Pikafish dev-20261026-9d7833aa`)
- **License:** GNU General Public License version 3 — see
  [Copying.txt](Copying.txt). The upstream README is preserved verbatim as
  [README.upstream.md](README.upstream.md); all other files are byte-identical
  copies of the upstream tree at the commit above (only build artifacts and
  binary files were omitted, see below).

Pikafish is a free, strong UCI xiangqi engine derived from Stockfish. It is
**not** developed here; this repo exists so that anyone who receives a binary
build of the engine from us (e.g. a WebAssembly build served by the game site)
can obtain the exact source it was built from, as GPLv3 requires. No changes
have been made to the engine source in this mirror. If we ever ship a modified
build, the modifications will be committed here before or with the release.

## Binary assets (not stored in this repo)

Binary files cannot be carried by our push tooling, and the NNUE weights are
additionally governed by their own license (below), so they are **not**
mirrored here. Obtain them from upstream:

| Asset | Source | Size | SHA-256 |
|---|---|---|---|
| `pikafish.nnue` (NNUE weights, "master-net") | https://github.com/official-pikafish/Networks/releases/download/master-net/pikafish.nnue | 50,241,100 bytes | `bf3982711150bf2f8b49e99cd93421320f6ca914cfcf131dad5eb7f2402573ac` |
| Official engine releases (binaries + docs) | https://github.com/official-pikafish/Pikafish/releases/latest (latest release at time of writing: Pikafish 2026.09.25) | — | per upstream release page |

For reference, the native Linux binary we built locally from this exact
source for benchmarking (`make -j build ARCH=x86-64-avx512`, 940,992 bytes)
has SHA-256 `7e15ac37c8bb2036252e4905457386e6d87f96ac313f41a1c87a28f54abd6590`.
It is not distributed; it only served local evaluation.

## A note on the NNUE weights license

The engine **source** is GPLv3, but the official `pikafish.nnue` weights are
**not** GPL: the Pikafish Networks repository
(https://github.com/official-pikafish/Networks) licenses them for legal,
**non-commercial use only, unless permission is granted**. Serving the weights
from a commercial website requires prior permission from the Pikafish team.
This mirror therefore covers the GPLv3 engine source only; it does not grant
any rights to the weights.

## Building

Native build (verified on Ubuntu 24.04, g++ 13.3, AVX-512 CPU):

```sh
cd src
# download pikafish.nnue next to the binary first (see link above), then:
make -j build ARCH=x86-64-avx512
./pikafish bench 64 1 13 default depth   # ≈ 499k nodes/s, 1 thread
```

WebAssembly build: **pending** — see [build/BUILD-NOTES.md](build/BUILD-NOTES.md).
When a WASM build is produced for the game site, its build script and any
patches will be added under `build/` in this repository.
