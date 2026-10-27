# Build notes

## Native build — VERIFIED (2026-10-26)

Environment: Ubuntu 24.04 VM, x86-64, g++ 13.3.0, CPU with AVX2/AVX-512/BMI2.

```sh
git clone https://github.com/official-pikafish/Pikafish.git   # commit 9d7833aa113626b3ea63ad83f49fb9b456bd5839
cd Pikafish/src
curl -skL -o pikafish.nnue https://github.com/official-pikafish/Networks/releases/download/master-net/pikafish.nnue
make -j build ARCH=x86-64-avx512
```

Results:

- Binary: `pikafish`, 940,992 bytes, identifies as `Pikafish dev-20261026-9d7833aa`.
- UCI smoke test: start position, `go depth 15` → bestmove `b2e2`,
  score cp 24, 78,842 nodes at **525,613 nodes/s** (1 thread).
- Tactical test FEN
  `r1bakr3/4a4/2ncb1n2/2p1p1p1p/p8/2PN5/P3P1P1P/C3B4/6Cc1/1RBAKA1NR w - - 0 1`,
  `go depth 15` → bestmove `d4c6`, score cp 88, **559,549 nodes/s**.
- `bench 64 1 13 default depth`: 1,516,563 nodes in 3,040 ms =
  **498,869 nodes/s** (1 thread).

## WebAssembly build — PENDING

No WASM build has been produced yet. Plan, based on how the Stockfish
ecosystem does it (lichess `stockfish-web`) and on existing community
Pikafish ports (e.g. the one powering mistboard.com):

- Compile with Emscripten from this source, single-threaded configuration
  first (no `SharedArrayBuffer`, so no COOP/COEP cross-origin isolation
  requirement on the game site); a multi-threaded variant would need
  cross-origin isolation and is not planned.
- The NNUE weights (~50 MB) would be fetched lazily by the browser only when
  a Pikafish-backed level is selected, and cached.
- Reminder: the weights' license forbids commercial use without permission
  (see the top-level README). The WASM engine binary itself is GPLv3; this
  repository is its corresponding-source home.
