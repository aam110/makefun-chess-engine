# ♞ Silowek — a 4 KB chess engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/aam110/makefun-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/aam110/makefun-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

**Silowek** is a chess engine squeezed into 4 KB of Python — small enough to
read in one sitting, strong enough to beat every rung of the practice gauntlet.
No clock, no randomness, no model calls: the same position always plays the
same move, so every trial replays byte-for-byte.

## The mate that sealed it — vs nodes-500k, Black to mate

Silowek (Black) finishes the strongest practice opponent with **…Rh1#**:

```
  a b c d e f g h
8 · · · · · · ♚ · 8
7 · · · · · · ♟ · 7
6 · ♟ · ♞ · · · · 6
5 ♟ · · ♙ · · · · 5
4 ♙ · · · · · ♕ ♟ 4
3 · · · · · ♙ · ♔ 3
2 ♖ ♙ · · ♛ · · · 2
1 · · · · · · · ♜ 1
  a b c d e f g h
```

White to move is mated: the rook on h1 checks down the file, the queen covers
g2, the h-pawn covers g3. No block, no capture, no escape.

## How it plays

| Piece of the engine | What it does |
|---|---|
| 0x88 board + piece lists | Fast move gen & eval |
| Alpha-beta + iterative deepening | Fixed 65,000-node budget per move |
| Quiescence | Captures, promotions, all evasions in check |
| Transposition table + killers | Ordering: TT move, MVV-LVA, killers |
| Null-move pruning + late-move reductions | Deeper search for the same nodes |
| Formula piece-square tables | Material + tapered king + bishop pair |

## Results (local practice trials)

| Trial | Score | Elo |
|---|---|---|
| trial-0 | 6–0, all by checkmate | 2,112 (max) |
| trial-1 | 3–1–2 | 1,364 |
| trial-2 | 4–0–2 | 1,451 |
| **Mean** | | **1,642** |

House baseline (sunfish 4K) mean: 1,365. Practice floor to enter the final:
1,154. On a custom top-rung gauntlet (nodes-500k/1M/2M) the engine draws
nodes-500k and loses to nodes-1M/2M — the final's hardest rungs.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

`main.py` is the whole engine — a packed LZMA stub produced by the kit's
`makefun compress`. Python 3.13 stdlib only.

---

<sub>Entered on [make.fun](https://make.fun/?ref=4kc4aac3&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
