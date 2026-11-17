[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zenganteng0987/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zenganteng0987/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# Tiny Chess

A chess engine in a single `main.py` of 4,040 bytes (limit 4,096) for the
make.fun `tiny-chess-01` bounty. The file is a minified Python program packed
as an LZMA payload with a small unpacking stub; it runs as
`python3 -P -s /bot/main.py` on Python 3.13, standard library only, and plays
as a client of the competition environment.

## How it plays

- 0x88 board, negamax search with quiescence, transposition table, PVS.
- Move ordering: TT move, MVV-LVA captures, killers, history.
- Null-move pruning and LMR; precomputed attack tables and inlined slider
  rays for speed (~30k nodes/second uncompressed-equivalent search).
- Evaluation: material plus piece-square tables for all six piece types.
  The pawn and king tables use the standard orientation — an earlier build
  of this engine carried them vertically flipped, which cost several
  hundred Elo; this entry carries the corrected tables.
- Fixed node budgets of 90k / 60k / 40k by game phase, chosen so the
  slowest move stays well under the 10-second per-move limit even on a
  slow CPU (worst measured think 6.5 s on a throttled VM).
- Every move is derived only from the position, the game moves and the
  trial seed: no wall clock and no unseeded randomness in the search.

## Local practice trials

Six games against the kit gauntlet (depth-2 855, depth-3 1073, depth-4 1199,
nodes-100k 1392, nodes-250k 1497, nodes-500k 1596), read from the trial
`state.json` files:

- Practice instance 0: 3.5 points (W depth-2, W depth-3, D depth-4,
  W nodes-500k) — practice Elo 1,364.
- Practice instance 2: 1.5 points — practice Elo 997.

The floor to enter the final is a practice Elo of 1,154, read from the
entry held at the deadline on the first practice instance.
