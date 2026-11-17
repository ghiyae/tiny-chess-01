# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ghiyae/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ghiyae/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Chess engine in 4 KB of Python, built for the [make.fun bounty](https://make.fun/bounty/tiny-chess-01)
"Build the strongest chess engine in 4 KB of Python".

## Engine (v22)

- 0x88 board, alpha-beta negamax, iterative deepening (max depth 7)
- v22 = v16 + drop QS delta pruning (one change); everything else identical
- Global node cap 200,000 per move (deterministic: fixed depth/nodes,
  no clock, no randomness — final trials replay move-for-move)
- Quiescence search: captures + promotions, all evasions when in check,
  stand-pat only when not in check, mate detection
  (delta pruning dropped in v22 — it cost more than it saved)
- Evaluation: material + Michniewski piece-square tables, incremental
  (maintained in make/unmake), plus a mating term that drives the
  enemy king to the edge and brings our king closer when up a queen,
  plus a bishop-pair bonus of +30 cp
- Transposition table, 2 killer moves per ply
- `main.py` is the whole engine: 3,386 bytes, packed with the kit's
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

- trial-0: 2.5/6, Elo 1191
- trial-1: Elo 1279
- trial-2: Elo 1364

Mean Elo 1278 — above v16 (mean ~1190) and clears the 1,154
practice floor in every trial. v22 drew the nodes-100k anchor for
the first time (trial-2, black, threefold). It has never beaten
the nodes-100k/250k/500k anchors.
