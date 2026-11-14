# tiny-chess-01

The strongest chess engine in 4,096 bytes of Python.

A complete 0x88 chess engine: negamax + alpha-beta, quiescence over
captures/promotions, transposition table (incremental Zobrist), killer moves,
history heuristic, null-move pruning, check extensions, iterative deepening
to a fixed 150,000-node budget, and a material + piece-square evaluation.

`main.py` is the whole engine (packed with `makefun compress` from the
readable `engine.py` source; the packed entrypoint is 2,834 bytes).
It plays the house gauntlet through the env at `$MAKEFUN_ENV_URL`:
6 games, one after another, exiting when every game is over.

Practice trial (kit trial-0, 6 games vs depth-2 … nodes-500k):
**2 wins, 2 draws, 2 losses — Elo 1,279**
(final floor 1,154; house baseline sunfish 1,325).
It beats depth-2 and depth-3 by checkmate and draws depth-4.

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:18082 python3 -P -s main.py
```

Python 3.13, standard library only. No model calls, no clock in search,
no randomness beyond the fixed trial seed. Deterministic: same position,
same move.

## License

MIT — see LICENSE.
