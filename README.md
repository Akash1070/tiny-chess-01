# tiny-chess-4k

[![make.fun tiny-chess-01](https://img.shields.io/badge/make.fun-tiny--chess--01-111111?style=flat&labelColor=000000)](https://make.fun/bounty/tiny-chess-01)
[![Built for make.fun](https://img.shields.io/badge/built%20for-make.fun-0055ff?style=flat)](https://make.fun/?ref=0t3v8mww)
[![Python 3.13](https://img.shields.io/badge/python-3.13-blue.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An ultra-compact, high-performance chess engine in **under 4 KB of Python** (4,079 bytes committed), built for make.fun's [`tiny-chess-01`](https://make.fun/bounty/tiny-chess-01) bounty.

Plays against the official 8-rung house gauntlet (`depth-2` to `nodes-2M`) using a calibrated, 100% deterministic node budget.

---

## ⚡ Core Engine Architecture

1. **Rotated Mailbox Board (10×12)**:
   - 180-degree board rotation (`rotate()`) standardizes move generation so moving side always plays North.
   - Eliminates redundant White/Black move-gen logic and cuts code footprint by 60%.

2. **Search Pipeline**:
   - **MTD-bi with Iterative Deepening**: Binary search with zero-width alpha-beta windows.
   - **Quiescence Search**: Tactical leaf resolution preventing horizon blunders.
   - **Transposition Table (TT)**: Fast bound caching across plies.
   - **Null-Move Pruning (NMP)** & **Late Move Reductions (LMR)**: Prunes non-critical quiet subtrees.
   - **Killer Heuristic & Move Ordering**: Searches TT best moves and tactical captures first.

3. **Positional Evaluation**:
   - **Procedural Piece-Square Tables (PST)**: Mathematically generated file/rank distance metrics.
   - **Tapered King Centralization**: Smoothly transitions king attraction to center as pieces leave the board.

4. **Strict Compliance**:
   - **4,079 bytes** committed (`main.py`), strictly under 4,096-byte hard cap.
   - Total repository tree strictly under 8,192 bytes.
   - Standard library Python 3.13 only (`os`, `sys`, `json`, `urllib.request`).
   - Bit-for-bit deterministic replay: no clocks in search.

---

## 🚀 Execution & Verification

Run against the make.fun environment:

```bash
export MAKEFUN_ENV_URL="http://127.0.0.1:8080"
python main.py
```

Tested and scored with make.fun's test harness:

```bash
makefun check
makefun submit --competition tiny-chess-01 --source .
```

---

Built for [make.fun](https://make.fun/?ref=0t3v8mww) • Wallet: `FkoaEHrqcKmCUjis98bYo7C7wcSgJZ7p8KXifpbLum9j`
