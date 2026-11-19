# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ghiyae/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ghiyae/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Chess engine in 4 KB of Python, built for the [make.fun bounty](https://make.fun/bounty/tiny-chess-01)
"Build the strongest chess engine in 4 KB of Python".

## Engine (v35c)

- 0x88 board, alpha-beta negamax, iterative deepening (max depth 7)
- v35c = v22 + aspiration windows (W=40) + commit-only-on-completion
  (only ever play a move from a fully completed depth — no partial-depth
  best move); node cap 100,000 per move (down from 200,000 — the 200k cap
  forfeited on time on slower hardware)
- Global node cap 100,000 per move (deterministic: fixed depth/nodes,
  no clock, no randomness — final trials replay move-for-move)
- Quiescence search: captures + promotions, all evasions when in check,
  stand-pat only when not in check, mate detection
- Evaluation: material + Michniewski piece-square tables, incremental
  (maintained in make/unmake), plus a mating term that drives the
  enemy king to the edge and brings our king closer when up a queen,
  plus a bishop-pair bonus of +30 cp
- Transposition table, 2 killer moves per ply
- `main.py` is the whole engine: 3,414 bytes, packed with the kit's
  makefun compress (LZMA) from the readable source. It reads, imports
  or execs no other file in the repository. Python 3.13, stdlib only.

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

## Size rule

- `main.py`: at most 4,096 bytes as committed.
- The rest of the tree (this README, LICENSE): at most 8,192 bytes
  in all, no file over 4,096. MIT licence.

## Practice results (local, real env server)

- trial-0 (CAP 100k): 4.5/6, Elo 1545 — W vs depth-2, W vs depth-3,
  W vs depth-4, W vs nodes-100k, D vs nodes-250k, L vs nodes-500k
- Selfplay vs v22 (10 games, CAP 20k): 12W 7D 1L, +215 Elo
- Timing (trial-0): mean 1.5 s/move, p95 2.4 s, max 3.4 s, 0 forfeits

v35c is the first version to beat the nodes-100k anchor and to clear
the 1,325 Elo pay bar locally (trial-0 Elo 1545, target 1400).
