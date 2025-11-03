# Protocol Levels for Physical Gameboard UI

This document outlines progressive capability levels for hardware using the [Protocol Description](./README.md). These levels allow implementations to start with minimal functionality and optionally expand toward more interactive and autonomous systems.

---

## Level 0 — Minimal I/O

**"Truly dumb" board with no internal state or move validation.**

- Sends pick-up (`-e2`) and put-down (`+e4`) messages
- Receives move instructions and LED indicators from an external system
- Does not verify move order or track game state
- Suitable for lightweight integrations or passive boards

📄 See: [Protocol Description](./README.md)

---

## Level 0.1 — User Move Verification

**Adds minimal intelligence to ensure the user follows instructions correctly.**

- Tracks individual move steps to verify user actions match instructions
- Can detect and reject incorrect sequences (e.g., invalid drop locations)
- Waits for expected input before proceeding
- Still does not understand game rules, turns, or board state

Use case: Enforces user compliance with server instructions

---

## Level 1 — State Synchronization

**Semi-intelligent board with internal board state memory.**

- Accepts a full board state from the server (`S...`) and guides the user to apply it
- Can respond with current board state (`R...`)
- Supports square-by-square diagnostics (`?e1`) and correction
- Can initiate a resynchronization process (e.g., via tap command)

Use case: Keeps the board in sync with external game logic, even after connection loss or user error

---

## And Beyond...

While beyond the intended scope of the minimal protocol, the following levels illustrate the upper limits of board autonomy.

---

## Level X — Full UCI-Compliant Board

**The board implements full game logic and legality checking internally.**

- Enforces move legality and tracks full game state (turns, castling, en passant, etc.)
- Recognizes check, checkmate, stalemate, and illegal moves
- Communicates using the UCI protocol or a compatible custom layer
- No external game logic required — the board becomes the authoritative controller

**Key capabilities:**
- Real-time legality validation
- Autonomous error detection
- UCI-style command interface (`uci`, `position`, `go`, etc.)

**Use cases:**
- Connecting directly to UCI-compatible engines (e.g., Stockfish)
- Running local or remote games without a dedicated app
- Acting as a "smart engine board" in distributed systems

---

## Level Y — Embedded Chess Computer

**Fully autonomous system with an embedded chess engine.**

The board includes an onboard chess engine (e.g., Stockfish), enabling it to play independently against a human player.

**Key capabilities:**
- Embedded chess engine running locally
- No need for external apps, servers, or game logic
- Local gameplay using lights, taps, or optional displays
- Optional setup and control via mobile app or web UI

**Typical features:**
- Selectable difficulty levels
- Move suggestions and evaluation
- Game history and undo support

Effectively, this level turns the board into a **standalone electronic chess computer**, using its physical interface for both input and feedback.

> Requires significantly more capable hardware (CPU, memory) than Levels 0–X.

---

## Notes

- Levels 0, 0.1, and 1 all use the same base message protocol.
- Levels are additive — each builds on the capabilities of the previous one.
- These tiers help clarify where features belong and allow implementations to grow gradually from minimal to advanced.
