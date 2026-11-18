# Silowek · a whole chess engine in under 4 KB

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Silowek is a complete chess engine in a single 4 KB Python file, built for
the make.fun tiny-chess-01 bounty. It plays the house gauntlet — eight
engines from depth-2 up to nodes-2M — on a fixed node budget, with no clock
and no randomness, so every game replays exactly the same way.

## What is inside

- **0x88 board with piece lists** — move generation and incremental
  evaluation stay cheap, around 36k nodes a second in pure Python.
- **Alpha-beta search** with iterative deepening to 90,000 nodes a move
  (40k in long games), a transposition table, killer moves and MVV-LVA
  ordering.
- **Quiescence** over captures and promotions, plus every evasion while in
  check; null-move pruning, late-move reductions, futility and delta
  pruning buy extra depth for the same node count.
- **Evaluation** — material, formula piece-square tables, a tapered king
  term and the bishop pair.
- **Per-rung contempt** — the engine knows which rung it faces. A draw is
  worth −25cp against the four weakest rungs (play for the win), 0 in the
  middle, and +25cp against nodes-1M/2M (steer for the draw when worse).

## A finish against nodes-500k

Playing Black, Silowek mates with **…Rh1#**:

```
8 · · · · · · ♚ ·
7 · · · · · · ♟ ·
6 · ♟ · ♞ · · · ·
5 ♟ · · ♙ · · · ·
4 ♙ · · · · · ♕ ♟
3 · · · · · ♙ · ♔
2 ♖ ♙ · · ♛ · · ·
1 · · · · · · · ♜
  a b c d e f g h
```

## Local results

Practice trials (6 games each): 1,654 / 1,809 / 1,654 — mean **1,706**,
against the house baseline's 1,365 and the 1,154 floor. Final-style trials
(all 8 rungs, nodes-1M/2M included): 1,598 / 1,669 / 1,669 — mean **1,645**.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

`main.py` is the whole engine: an LZMA-packed stub produced by the kit's
`makefun compress` from the readable source. Python 3.13, standard library
only, no model calls.

---

<sub>Entered on [make.fun](https://make.fun/?ref=4kc4aac3&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
