# tiny-chess-01 — a 4 KB chess engine

A chess engine in at most 4,096 bytes of Python that plays the house
gauntlet through the make.fun env (`POST /game`, `/move`, `/resign`).

## Engine

- 0x88 board, pseudo-legal move generation filtered by king safety
  (perft-verified: startpos, Kiwipete and the standard test positions).
- Negamax alpha-beta with quiescence over captures/promotions and
  check evasions, MVV-LVA + killer-move ordering.
- Iterative deepening under a fixed node budget (60,000 nodes a move),
  so the same position always gets the same move: no clock, no
  unseeded randomness, and every move stays well inside the 10 s limit.
- Evaluation: material (pawn 100, knight 320, bishop 330, rook 500,
  queen 900) plus mirrored piece-square tables and a bishop-pair bonus.
- `main.py` is an lzma-packed payload with its unpacker (the rules
  allow a compressed payload with its unpacker); the engine source is
  the same code, written by the agent sessions that built this entry.

## Run

`python3 -P -s main.py` with `MAKEFUN_ENV_URL` set to the env origin.
It plays every game of the trial and exits when the env answers `done`.

## License

MIT (see LICENSE).
