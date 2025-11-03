# Protocol Levels for Physical Gameboard UI

This document outlines progressive capability levels for hardware using the [Protocol Description](./README.md). The levels allow implementations to start with minimal functionality and optionally expand toward more interactive and state-aware systems.

---

## Level 0 — Minimal I/O

**"Truly dumb" board with no state memory or move validation.**

- Sends pick-up (`-e2`) and put-down (`+e4`) messages
- Receives move instructions and LED indicators from external system
- Does not verify move order or track game state
- Suitable for most lightweight integrations

📄 See: [Protocol Description](./README.md)

---

## Level 0.1 — User Move Verification

**Adds minimal intelligence to ensure user follows instructions correctly.**

- Tracks individual move steps to verify user performs expected action
- Can reject illegal sequences (e.g. incorrect drop location)
- Delays further steps until expected input is complete
- Still no game rules, turn tracking, or internal board state

Use case: Enforces user compliance with server instructions

---

## Level 1 — State Synchronization

**Semi-intelligent board that keeps track of its internal state.**

- Accepts full board state from server (`S...`) and guides user through applying it
- Can respond with current board state (`R...`)
- Supports square-by-square diagnostics (`?e1`) and correction
- Can initiate resync process (e.g. via tap command)

Use case: Ensures gameboard is always in sync with external game logic, even after connection loss or error

---

## Notes

- All levels use the same base message protocol
- Levels are additive: each level builds on the functionality of the previous one
- Documenting these tiers helps separate minimal hardware support from extended features
