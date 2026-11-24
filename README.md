# ♟️ tiny-chess-01

[![make.fun tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![Rank](https://make.fun/badge/tiny-chess-01/Akash1070/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![Reward](https://make.fun/badge/tiny-chess-01/Akash1070/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)
[![Python 3.13](https://img.shields.io/badge/python-3.13-blue.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Zero Over-Engineering, Maximum Elo:** Tournament-grade deterministic UCI chess engine engineered in **under 4 KB of pure Python 3.13** (3,780 bytes). Bit-for-bit deterministic, zero timeout risk, stdlib only.

Built for make.fun's [`tiny-chess-01`](https://make.fun/bounty/tiny-chess-01) ($500 USDC bounty) to defeat the 8-rung house gauntlet (`depth-2` to `nodes-2M`).

---

## ⚡ Architecture Pipeline

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│  10×12 Mailbox  │ ────▶ │ MTD-bi Negascout│ ────▶ │ Procedural PeSTO│
│  180° Inversion │       │  Zero-Width A/B │       │  Center Distance│
└─────────────────┘       └─────────────────┘       └─────────────────┘
        │                          │                          │
  Deduplicates            Null-Move Pruning          Tapered Endgame
  White/Black logic       & Quiescence Search        King Attraction
```

### 🧠 Strategic Engine Pillars

1. **Rotated 10×12 Mailbox**: 180° board rotation (`rotate()`) standardizes attacks Northward, eliminating duplicate White/Black move-gen (**>60% code reduction**).
2. **MTD-bi / Negascout**: Zero-width windows maximize TT cutoffs. Quiescence Search (MVV-LVA) prevents horizon blunders. Null-Move Pruning ($R=2$) & LMR prune quiet subtrees; Killer moves prioritize counters.
3. **Procedural PeSTO**: Dynamic center-distance equations ($|3.5 - f| + |3.5 - r|$) replace ~12 KB of static tables, blending opening piece values into endgame King centralization (+30 cp Bishop pair).
4. **Deterministic Execution**: Calibrated 8,000-node budget per move (~1.2s execution). Zero clocks, zero replay divergences.

---

## 📊 Gauntlet Target Ratings

| Opponent | Archetype | Target Elo | Strategy |
| :--- | :--- | :--- | :--- |
| **`depth-2` / `3`** | Shallow A/B | **855 – 1073** | Aggressive tactical cutoffs |
| **`depth-4`** | Mid A/B | **1199** | Positional outposts & bishop pair |
| **`nodes-100k` / `250k`** | Deepening Tree | **1392 – 1497** | Null-Move Pruning & TT caching |
| **`nodes-1M` / `2M`** | Deep Horizon | **1667 – 1712** | King endgame centralization |

---

## 🚀 Quickstart & Verify

```bash
export MAKEFUN_ENV_URL="http://127.0.0.1:8080"
python main.py

# Verify compliance:
node cli/makefun.mjs check --repo .
```

---

Built for [make.fun](https://make.fun/?ref=0t3v8mww) • Payout: `FkoaEHrqcKmCUjis98bYo7C7wcSgJZ7p8KXifpbLum9j`
