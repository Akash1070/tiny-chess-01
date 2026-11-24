# tiny-chess-01

[![make.fun tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![Rank](https://make.fun/badge/tiny-chess-01/Akash1070/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![Reward](https://make.fun/badge/tiny-chess-01/Akash1070/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)
[![Python 3.13](https://img.shields.io/badge/python-3.13-blue.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An ultra-compact tournament-grade deterministic UCI chess engine engineered in **under 4 KB of pure Python 3.13** (3,780 bytes), built for make.fun's [`tiny-chess-01`](https://make.fun/bounty/tiny-chess-01) bounty ($500 USDC).

Plays against the official 8-engine house gauntlet (`depth-2` to `nodes-2M`) utilizing an exact 8,000-node deterministic budget.

---

## ⚡ Architecture Highlights

### 1. Rotated 10×12 Mailbox Board
- **180° Inversion**: Flipping the board (`rotate()`) after each ply standardizes attacks Northward.
- **Code Deduplication**: Eliminates duplicate White/Black move-gen and asymmetric pawn logic, saving >60% footprint.

### 2. Search & Tactical Pipeline
- **MTD-bi / Negascout**: Binary root search with zero-width alpha-beta testing windows.
- **Iterative Deepening**: Progressively searches plies within an 8,000-node hard limit per move.
- **Quiescence Search**: Resolves tactical captures and promotions to prevent horizon blunders.
- **Pruning Heuristics**: Null-Move Pruning (NMP, $R=2$) and Late Move Reductions (LMR) prune quiet subtrees.
- **Transposition & Ordering**: Hash bounds caching, killer moves (2/ply), and MVV-LVA capture prioritization.

### 3. Procedural Evaluation
- **Procedural PeSTO PSTs**: Piece-square table bonuses generated dynamically using center-distance equations ($|3.5 - f| + |3.5 - r|$).
- **Tapered Endgame King Centralization**: Smoothly transitions King weight toward center as pieces trade.
- **Bishop Pair Bonus**: Retains bishop pair (+30 cp).

### 4. Determinism & Compliance
- **Zero Clock Calls**: Node-bounded search guarantees bit-for-bit identical replays.
- **Python 3.13 stdlib only**: `os`, `sys`, `json`, `urllib.request`. Zero external dependencies.
- **Strict Size Limits**: `main.py` is 3,780 bytes ($\le 4,096$); total repo is $<8,192$ bytes.

---

## 🚀 Quickstart

Run against make.fun trial environment:

```bash
export MAKEFUN_ENV_URL="http://127.0.0.1:8080"
python main.py
```

Run compliance checks:

```bash
node cli/makefun.mjs check --repo .
```

---

Built for [make.fun](https://make.fun/?ref=0t3v8mww) • Payout: `FkoaEHrqcKmCUjis98bYo7C7wcSgJZ7p8KXifpbLum9j`
