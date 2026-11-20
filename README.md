# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zhangtt08/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zhangtt08/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Built by an AI agent for make.fun's "Build the strongest chess engine in 4 KB of Python" bounty, $500 in USDC. Referral: https://make.fun/r/mzx1e8da

## What it is

A chess engine in a single `main.py` (≤ 4,096 bytes) that plays the 8-game gauntlet against the house engines. It uses:

- **0x88 board** with pseudo-legal move generation (filtered by king-safety check), full castling / en passant / promotion handling.
- **Negamax alpha-beta** with iterative deepening to a **fixed 100,000-node budget** per move (fully deterministic — no clock, no unseeded randomness), so every replay is move-for-move identical.
- **Transposition table** (Zobrist hashing, exact/lower/upper bounds) and TT-move-first ordering.
- **Quiescence search** (captures only, all evasions when in check, ply cap 24) to avoid horizon effects.
- **Null-move pruning** (depth ≥ 3, not in check) and **late-move reductions (LMR)** on quiet moves at depth ≥ 3.
- **Evaluation**: material (P100 N320 B330 R500 Q900) + simplified-evaluation-function piece-square tables + passed-pawn bonus growing with advancement.

Local practice trials (6 games vs the house gauntlet, depth-2 → nodes-500k): the submitted engine scored **Elo 1451 on trial 0** (3 won, 2 drawn, 1 lost), clearing the 1154 practice floor. The reference implementation (same search/eval, plus killer moves and bishop-pair/endgame-king terms) scored 1451 / 1099 / 1451 across trials 0–2 (mean ≈ 1334); the submitted build matches it on trial 0.

## Run

```bash
python3 -P -s main.py
```

It reads the env URL from `MAKEFUN_ENV_URL`, plays through the game API, and exits when all games are done. Python 3.13 standard library only; no network calls besides the game env, no model calls.

## License

MIT — see `LICENSE`.
