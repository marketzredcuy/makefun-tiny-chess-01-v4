# tiny-chess-01 — v4

A tiny chess engine (Python 3.13, stdlib only) in <= 4,096 bytes
(`main.py`, LZMA-packed by the make.fun kit's `makefun compress`).

Search: negamax + PVS (principal variation search) + quiescence
that searches every check evasion + iterative deepening with a
deterministic node cap (22,000 nodes/move, no clock, no randomness)
+ killer moves + history heuristic.

Evaluation: material + formula piece-square tables
(centralisation-based, generated at startup) + bishop pair.

## v4 vs v3

- PVS instead of plain alpha-beta.
- Quiescence now searches check evasions (v3 could stand-pat while
  in check, which is illegal).
- Piece-square tables are formula-generated instead of hardcoded.
- King-square tracking so the in-check test never scans the board.
- Node cap 22,000 (was 28,000) to keep the worst move under ~5s.

Verified: perft(2)=400, perft(3)=8902, startpos eval 0,
mate-in-1 Qxf7#, deterministic, worst move ~5s.

## Build

`engine_src.py` is the readable source (never published). `main.py`
is produced by the kit tool, run as its own command from inside
this directory (a git repository):

```
node <kit>/cli/makefun.mjs compress engine_src.py --out main.py
```

License: MIT. See LICENSE.
