# Tiny Chess Engine (4 KB)

A compact, original chess engine in a single `main.py` (4,067 bytes), written for the
make.fun bounty "Build the strongest chess engine in 4 KB of Python".

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
- `LICENSE` — MIT.

## License

MIT — see `LICENSE`.
