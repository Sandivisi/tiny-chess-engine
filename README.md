# tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Sandivisi/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Sandivisi/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A compact, original chess engine in a single `main.py` (3,242 bytes) for the make.fun bounty "Build the strongest chess engine in 4 KB of Python" — 0x88 negamax with alpha-beta, transposition table, quiescence search, and a 120,000-node deterministic move budget. Practice trials averaged Elo 1344 against the house gauntlet.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:18200 python3 -P -s main.py
```

The engine talks to the make.fun environment alone over HTTP (`POST $MAKEFUN_ENV_URL/game`, `/move`, `/resign`) and plays UCI moves. `engine.py` beside it is the readable source of the same engine.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

## Design

- **Board:** 0x88 mailbox, signed pieces (white +1..+6, black -1..-6).
- **Search:** negamax with alpha-beta, iterative deepening to a fixed total node
  budget per move (120,000 nodes — deterministic, no wall clock), transposition
  table (Zobrist hashing, fixed-seed PRNG), killer moves, MVV-LVA capture
  ordering, check extensions, and quiescence search over captures, promotions,
  and check evasions.
- **Evaluation:** incremental material + piece-square tables (formula-generated),
  tapered king safety (middlegame/endgame), and a bishop-pair bonus. Repetition
  and the fifty-move rule are handled inside the search.
- **Protocol:** talks to the make.fun environment alone over HTTP
  (`POST $MAKEFUN_ENV_URL/game`, `/move`, `/resign`), plays UCI moves.

## Strength

Practice trials against the house gauntlet (mean Elo over trials 0–2):

| Trial | Elo | W–D–L |
|-------|-----|-------|
| 0 | 1099 | 1–2–3 |
| 1 | 1654 | 5–0–1 |
| 2 | 1279 | 3–0–3 |
| **Mean** | **1344** | |

Beats the depth-2/3/4 rungs and scores against the 100k/250k-node rungs.

## Layout

- `main.py` — the whole engine. It is a compressed payload plus a small
  unpacker (`zlib` + base85), which together count toward the 4,096-byte cap.
  The engine itself is original code written for this bounty; it reads, imports,
  and execs no other file of the repo.
- `engine.py` — the readable source of the same engine (9,368 bytes).
- `LICENSE` — MIT.

## License

MIT — see `LICENSE`.
