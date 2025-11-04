# Chessboard User Interface

## Indicator Lights

Each square on the board contains one or more RGB LEDs (typically 1 or 4), which are used to indicate specific actions. Current light patterns are:

- **Slow blinking red** — Pick up the piece from this square
- **Slow blinking green** — Place a piece on this square
- **Fast blinking red** — Invalid move: wrong piece picked up or dropped in an illegal location
- **Fast blinking blue** — Place the indicated piece here (e.g., during setup or promotion)

---

## Moving the Pieces Properly

Since the board cannot identify pieces by type, only detect their presence or absence, there are a few guidelines for how to move them:

- Move pieces **clearly and decisively** — avoid hesitation or wiggling, which could lead to false readings or ambiguous moves.
- Place the piece **in the center of the square** to ensure detection.
- Avoid sliding pieces between squares, as this may confuse the detection logic.

### Special cases requiring move order

Some moves in chess require special attention to the **order of actions**, especially on a board that doesn't track piece identity:

- **Capturing** — First pick up **your own piece**, then the captured piece, and finally place your piece in the target square.  
  > *(Technically this could be handled in software if done in the wrong order, but better to do it right 😉)*

- **Castling** — Move the **king first**, then the rook. The reverse order may be interpreted as two separate moves.

- **Promotion** — The software must be informed which piece the pawn promotes to. This can be indicated through external controls or predefined regions on the board (see below).

---

## Board Initialization

When the board is reset (either via power cycle or a **triple-tap reset**, see below), it will check its state and guide the player to remove, move, or add pieces to match the desired starting position.

If the board was already in the correct initial setup or in a preserved state from the previous game, no changes are needed and play can begin immediately.

### Removing Pieces

Pick up pieces from the **indicated squares** (see [Indicator Lights](#indicator-lights)).

### Moving Pieces

Move a piece from one square to another as instructed by the indicators.

### Adding Pieces

To add a piece, pick it up from outside the board and place it on the **indicated square**.

After the initial setup, new pieces can also be added during play by first “identifying” the piece (see below). If the placement square is also the final destination, the piece is placed directly.

---

## Identifying the Piece Type

Special regions on the board are used to **identify the piece type and color** during setup or when adding new pieces. When a piece is placed in these regions, it will be interpreted as follows:

| File/Rank     | Piece Identified         |
|---------------|---------------------------|
| Rank 2        | White pawn                |
| Rank 7        | Black pawn                |
| File a or h   | Ranks 1–4: White rook<br>Ranks 5–8: Black rook |
| File b or g   | Ranks 1–4: White knight<br>Ranks 5–8: Black knight |
| File c or f   | Ranks 1–4: White bishop<br>Ranks 5–8: Black bishop |
| File d        | Ranks 1–4: White queen<br>Ranks 5–8: Black queen |
| File e        | Ranks 1–4: White king<br>Ranks 5–8: Black king |

---

## Tap-Based Gestures Using Pieces

Some special board actions can be triggered by briefly lifting and replacing a piece — a "tap gesture." These are not normal moves, but intentional inputs interpreted by the system in specific situations.

### Promotion Piece Selection

When a pawn reaches the 8th rank, the player must select the promoted piece.

**To do this:**
1. Pick up the desired promotion piece from outside the board.
2. Tap it briefly on a designated [identification square](#identifying-the-piece) to identify its type.
3. Then place it on the promotion square.

This gesture tells the system which piece to use for the promotion, without needing external buttons or menus.

### Choosing Who Starts the Game

To give the **first move** to the opponent (black), the player can indicate this at the beginning of the game:

- Lift the **black queen** briefly
- Place it back to its starting square (`d8`)

The system interprets this as a signal to let the opponent start.

---

## Tap-Based Controls

Some basic board-level commands are available via tapping the sides of the board. These are detected by onboard sensors (e.g., accelerometers or capacitive edge detection).

### Tap Side Selection  
*TBD: Description of how tap on side selects color, side to play, or other option.*

### Double-Tap Setup  
*TBD: Description of how a double tap puts the board into setup mode.*

### Triple-Tap Reset  
Triggers a full board reset. The board will reinitialize and guide the player through removing or re-adding pieces as needed.

---

## 📄 License

This documentation is licensed under the  
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).  
See the [LICENSE](./LICENSE) file for full details.
