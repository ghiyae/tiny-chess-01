# tiny-chess-01

Chess engine in 4 KB of Python, built for the make.fun bounty
"Build the strongest chess engine in 4 KB of Python".

## Engine (v6)

- 0x88 board, alpha-beta negamax, iterative deepening (max depth 5)
- Global node cap 100,000 per move (deterministic: fixed depth/nodes,
  no clock, no randomness — final trials replay move-for-move)
- Quiescence search: captures + promotions, all evasions when in check,
  stand-pat only when not in check, mate detection, delta pruning,
  bad-capture pruning
- Evaluation: material + Michniewski piece-square tables, incremental
  (maintained in make/unmake), plus a mating term that drives the
  enemy king to the edge and brings our king closer when up a queen
- Transposition table, 2 killer moves per ply, repetition detection
  (returns 0 on third occurrence)
- `main.py` is the whole engine: 3,407 bytes, packed with
  `makefun compress` from the readable source. It reads, imports or
  execs no other file in the repository. Python 3.13, stdlib only.

## Size rule

- `main.py`: at most 4,096 bytes as committed.
- The rest of the tree (this README, LICENSE): at most 8,192 bytes
  in all, no file over 4,096. MIT licence.

## Practice results (local, real env server)

- trial-0: 1W 2D 3L, Elo 1099
- trial-1: 0W 0D 6L, Elo 0
- trial-2: 1W 1D 4L, Elo 997

True Elo is roughly 900–1,100 — below the 1,154 practice floor, but
submitted as the best version available before the deadline.
