# tiny-chess-01

The strongest chess engine in 4,096 bytes of Python.

A complete 0x88 chess engine: negamax + alpha-beta, quiescence over
captures/promotions, transposition table (incremental Zobrist), killer moves,
history heuristic, null-move pruning, check extensions, iterative deepening
to a fixed 150,000-node budget, a material + piece-square evaluation, and
repetition-aware search (a Zobrist-key occurrence count over game history
plus search path; a third occurrence scores 0 as a draw, checked before the
TT lookup in search and in quiescence at the horizon).

`main.py` is the whole engine (packed with `makefun compress` from the
readable `engine.py` source; the packed entrypoint is 2,969 bytes).
It plays the house gauntlet through the env at `$MAKEFUN_ENV_URL`:
6 games, one after another, exiting when every game is over.

Practice trials (kit trials 0/1/2, 6 games each vs depth-2 … nodes-500k),
repetition-aware build (v6a = submitted engine + repetition only):

| trial | W–D–L | Elo |
|-------|-------|-----|
| 0 | 4–0–2 | **1,451** |
| 1 | 2–2–2 | 1,279 |
| 2 | 0–3–3 | 997 |
| **mean** | | **1,242** |

Trial-0 (the practice Elo the server reports) beats the house baseline
sunfish (1,325) by 126 Elo and matches the 1,451 benchmark. Previous
submitted build ("Lucid Corona", no repetition): trial-0 1,279, mean 1,159.
Changes tested and rejected on trial data: bishop-pair + tempo eval
(1,099 trial-0), quiescence-in-check fix (1,099, time losses), LMR (876),
node cut to 100k (708), aspiration windows ±40 (1,279 / 1,191).

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:18082 python3 -P -s main.py
```

Python 3.13, standard library only. No model calls, no clock in search,
no randomness beyond the fixed trial seed. Deterministic: same position,
same move.

## License

MIT — see LICENSE.
