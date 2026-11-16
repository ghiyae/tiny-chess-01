# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ghiyae/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ghiyae/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Chess engine in 4 KB of Python, built for the [make.fun bounty](https://make.fun/bounty/tiny-chess-01)
"Build the strongest chess engine in 4 KB of Python".

## Engine (v7)

- 0x88 board, alpha-beta negamax, iterative deepening (max depth 6)
- Global node cap 200,000 per move (deterministic: fixed depth/nodes,
  no clock, no randomness — final trials replay move-for-move)
- Quiescence search: captures + promotions, all evasions when in check,
  stand-pat only when not in check, mate detection, delta pruning,
  bad-capture pruning
- Evaluation: material + Michniewski piece-square tables, incremental
  (maintained in make/unmake), plus a mating term that drives the
  enemy king to the edge and brings our king closer when up a queen
- Transposition table, 2 killer moves per ply
- `main.py` is the whole engine: 4,062 bytes, packed (LZMA + base85)
  from the readable source. It reads, imports or execs no other file
  in the repository. Python 3.13, stdlib only.

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

## Size rule

- `main.py`: at most 4,096 bytes as committed.
- The rest of the tree (this README, LICENSE): at most 8,192 bytes
  in all, no file over 4,096. MIT licence.

## Practice results (local, real env server)

- trial-0: 2W 0D 4L, Elo 1099
- trial-1: 1W 0D 5L, Elo 876
- trial-2: 1W 0D 5L, Elo 876

Mean Elo ~950 — below the 1,154 practice floor, but stronger than
v6b (mean ~699; trial-0 1099, trial-1 0, trial-2 997). v7 beats the
depth-4 anchor, which v6 never did.
