# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/jjb81620101-cloud/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/jjb81620101-cloud/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in under 4 KB of Python: a 0x88 board, alpha-beta search with quiescence and iterative deepening, piece-square-table evaluation, and a fixed node budget so every move lands well inside the time limit. The readable source is packed into `main.py` with the kit's `makefun compress` (LZMA); `main.py` is 2,330 bytes.

## Run it

```sh
MAKEFUN_ENV_URL=http://localhost:8000 python3 main.py
```

The grader runs it as `python3 -P -s /bot/main.py` against the make.fun game environment (`POST /game`, `POST /move`, `POST /resign`). It plays 8 games against the house gauntlet, alternating colours, from the book openings the environment supplies.

## How it plays

- **Board:** 0x88 (128 squares, off-board detected by `sq & 0x88`), pieces 1–6, positive white / negative black.
- **Move generation:** pseudo-legal generation (pawns with double push, en passant and promotions; knights; king with castling; sliding pieces), then a legality filter that rejects any move leaving the own king attacked.
- **Evaluation:** material (P100 N320 B330 R500 Q900) + piece-square tables (halved, packed as printable characters, doubled back in the evaluator) + a bishop-pair bonus.
- **Search:** negamax alpha-beta with quiescence over captures and promotions, MVV-LVA ordering, and iterative deepening (depth 1–6) under a fixed 40,000-node budget — the deepest fully completed depth supplies the move. Deterministic: no wall clock, no unseeded randomness; the same position always yields the same move.

---

Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards
