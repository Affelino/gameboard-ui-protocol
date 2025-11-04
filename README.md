# Gameboard UI Protocol

*A minimal text-based protocol for controlling physical gameboards — with square-level piece detection and RGB LED indicators.*

This protocol enables boards of varying intelligence levels to act as a user interface for digital board games such as chess or checkers.  
Even the simplest implementation — a "dumb" board with no internal state — can meaningfully interact with an external game system.

More advanced boards can add features like verifying user actions, maintaining internal board state, or guiding the player through error correction.  
Depending on the implementation level, some of the protocol messages may not be required.

📄 For an overview of possible capability levels, see [ProtocolLevels.md](./ProtocolLevels.md).  
📄 Special considerations of using simple protocol like this to implement chess UI, see [GameboardUserInterface.md](./GameboardUserInterface.md).

Part of the **NoEi project series**.

---

## 🧩 Overview

This document defines a lightweight communication protocol between a physical gameboard and an external controller or application.

The protocol is designed for minimal hardware that:

- Detects whether a square is occupied or empty
- Cannot identify piece type or check move legality
- Controls LED indicators on the board

All game logic (e.g. move legality, promotion, turn handling) is handled externally.

---

## 🔁 Message Flow

### Server → Board

#### During Game

- **Inform pick-up** (1)  
- **Inform put-down** (2)  
- **Indicate possible move targets** (3)  
- **Warn about invalid move** (4)  
- **Indicate which piece to place** (6)

#### During Setup

- **From starting position:** pick-up (1), put-down (2)  
- **From empty board:** indicate piece (6), put-down (2)

#### Misc

- **Clear indicator(s)** (5)  
- **Set or query board state** (7)  
- **Query square status** (8)

---

### Board → Server

- **Report pick-up / put-down** (1/2)  
- **Report full board state** (7)  
- **Report square status & piece count** (9)

---

## 💬 Message Format

Each message is a short ASCII string ending with `CR (\r)`.  
Multiple messages can be sent on the same line, separated by whitespace.

---

## 🔤 Message Examples

### Generic (Both Directions)

| Message | Description      | Example |
|---------|------------------|---------|
|    1    | Pick-up event    | `-f2`   |
|    2    | Put-down event   | `+f4`   |

---

### Server → Board

| Message | Description                    | Example                            |
|---------|--------------------------------|------------------------------------|
|    3    | Indicate valid moves           | `*f3`                              |
|    4    | Invalid move warning           | `!f5`                              |
|    5    | Clear indicators               | `.f4`, or just `.` for all squares |
|    6    | Indicate piece to place        | `^e1`                              |
|    7    | Set board state                | `S<position>`                      |
|    8    | Query square status            | `?e1`                              |

Example for full board setup:  
```
SRNBQKBNRPPPPPPPP................................pppppppprnbqkbnr
```

---

### Board → Server

| Message | Description                    | Example                                                            |
|---------|--------------------------------|--------------------------------------------------------------------|
|    7    | Report board state             | `RNBQKBNRPPPPPPPP................................pppppppprnbqkbnr` |
|    9    | Report square + piece count    | `#30` (hex)                                                        |

#### Hex Code Explanation (`#xx`)

This is a bitmask used to encode multiple states in a compact reply:

- `0x1F` – Number of pieces on the board, excluding the square that was queried (max 31)
- `0x20` – Occupied flag for the requested square (1 = yes, 0 = no)
- `0xC0` – Reserved (future use)

Example:  
`#3F` →  
- 0x1F = 31 pieces
- 0x20 = square is occupied  
- Reserved = 0

---

## ✅ Status & Compatibility

- Designed for use with physical chessboards using magnets + Hall sensors
- Compatible with microcontrollers (e.g. ESP-01) using I²C and LED control
- Easily extendable to other board games using square-based movement
- Readable and testable manually or with scripting tools

---

## 📄 License

This documentation is licensed under the  
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).  
See the [LICENSE](./LICENSE) file for full details.

---

## 📬 Feedback

Have thoughts, ideas, or want to adapt this for your project?  
Feel free to [open an issue](https://github.com/YOUR-REPO/issues) or reach out.

---
