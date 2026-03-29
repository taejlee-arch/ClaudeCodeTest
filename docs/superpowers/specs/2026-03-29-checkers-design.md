# Checkers Game Design

**Date:** 2026-03-29
**File:** `checkers.html`
**Mode:** 2-player local (same screen, alternating turns)

---

## Architecture

Single self-contained `checkers.html` file. No build step, no dependencies. Follows the same structure as `index.html` (inline CSS + inline JS + minimal HTML).

### State

| Variable | Type | Description |
|---|---|---|
| `board` | `Array[8][8]` | Each cell is `null` or `{ p: 1\|2, king: bool }` |
| `turn` | `1\|2` | Which player's turn it is |
| `selected` | `{r, c} \| null` | Currently selected piece (row/col) |
| `mustJumps` | `Array<{r,c}>` | Pieces that must jump this turn (mandatory jump enforcement) |
| `gameOver` | `bool` | Whether the game has ended |

### Key Functions

- `init()` — Reset board to starting positions, Player 1 top rows, Player 2 bottom rows
- `render()` — Redraw entire board from state
- `getValidMoves(r, c)` — Return list of `{toR, toC, jumpR?, jumpC?}` for a piece
- `getJumpsForPlayer(player)` — Scan all pieces for available jumps (for mandatory jump check)
- `handleCellClick(r, c)` — Handle click: select piece, or execute move
- `applyMove(from, to)` — Update board state, handle captures, check king promotion, check win
- `switchTurn()` — Change `turn`, compute new `mustJumps`, update status
- `checkWin(player)` — Return true if opponent has no pieces or no legal moves

---

## Rules

- **Board:** 8×8, pieces only on dark squares (where `(row + col) % 2 === 1`)
- **Starting positions:** Player 1 (red) occupies rows 0–2 on dark squares; Player 2 (teal) occupies rows 5–7 on dark squares
- **Movement direction:** Player 1 moves downward (increasing row); Player 2 moves upward (decreasing row); kings move in all 4 diagonal directions
- **Simple move:** One step diagonally to an adjacent empty dark square
- **Jump:** Leap over an adjacent opponent piece to the empty square beyond; the jumped piece is captured and removed
- **Mandatory jumps:** If any jump is available for the current player, only jump moves are legal that turn
- **Chain jumps:** After a jump, if the same piece can jump again, the player must continue jumping with that piece (turn does not switch until no further jumps are possible)
- **King promotion:** A piece reaching the opponent's back row (row 7 for Player 1, row 0 for Player 2) becomes a king; a newly promoted king cannot continue a chain jump on that same turn
- **Win condition:** A player wins when the opponent has no pieces remaining or has no legal moves

---

## UI

### Visual Style

Matches `index.html` dark theme:

| Element | Color |
|---|---|
| Page background | `#1a1a2e` |
| Dark squares (playable) | `#16213e` |
| Light squares (non-playable) | `#0f3460` |
| Player 1 pieces | `#e94560` (red) |
| Player 2 pieces | `#a8dadc` (teal) |
| Selected piece highlight | bright border/glow |
| Valid move indicator | subtle dot or highlight on destination square |
| Win highlight | glow matching piece color |

### Layout

- Centered on page, same flex column layout as `index.html`
- `<h1>CHECKERS</h1>` at top
- Status bar below title showing whose turn it is or the winner
- 8×8 CSS Grid board
- "Restart" button below board

### Interaction

1. Click a piece belonging to the current player → selects it (highlights it, shows valid destinations)
2. Click a valid destination → executes the move
3. Click a different own piece → switches selection
4. Click an invalid square → deselects
5. After a jump, if chain jump is available, the same piece stays selected and only its jump destinations are shown; other pieces cannot be selected until the chain is complete

### No sound

No audio effects (unlike tic-tac-toe, checkers has no emoji theme driving sound design).

---

## Out of Scope

- AI opponent
- Move timer
- Piece count display
- Move history / undo
- Animations beyond CSS transitions
