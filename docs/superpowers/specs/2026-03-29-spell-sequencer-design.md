# Spell Sequencer — Design Spec
_Date: 2026-03-29_

## Overview

A single-file browser puzzle game (`index.html`) for a 12-year-old with some Scratch/Roblox experience. Fantasy theme. The game is fun first, educational second — coding concepts are revealed after each puzzle is solved, never blocking play.

## Target Audience

- Age 12, familiar with block-based coding (Scratch/Roblox)
- Goal: spark curiosity about real code (JavaScript), not teach it formally

---

## Core Game Loop

1. Player sees a level with a **target recipe** — the correct sequence of rune tiles (e.g., Fire → Water → Earth)
2. A **pool tray** at the bottom shows all available runes (recipe runes + distractors)
3. Player drags runes into numbered **casting slots** to match the recipe
4. Player hits **Cast Spell**
5. **If correct:** success animation plays (glow, particle effect), then the **Wizard's Tome** panel slides up showing 4–6 annotated lines of real JavaScript
6. **If wrong:** slots shake, gentle "try again" — no penalty, no lives
7. Player dismisses the Tome and advances to the next level

---

## Levels (8 total)

Each level introduces one coding concept, revealed in the Tome after success:

| Level | Concept | Tome teaches |
|-------|---------|-------------|
| 1–2 | Arrays | A rune sequence is a JS array: `["fire", "water", "earth"]` |
| 3–4 | Loops | Casting each rune uses a `for` loop |
| 5–6 | Conditionals | `if` checks whether the rune matches before casting |
| 7–8 | Functions | A named spell = a reusable function |

Slots per level: 3 (levels 1–4), 4 (levels 5–6), 5 (levels 7–8).

---

## UI Layout

Single screen, no navigation:

- **Top bar:** Title ("Spell Sequencer"), level counter ("Spell 3 of 8"), flavour objective text
- **Center:** Casting board — numbered slots in a row, "Cast Spell" button below; target recipe shown to the right as rune icons + names
- **Bottom tray:** Draggable rune tiles (pool)
- **Tome overlay:** After success — dark translucent panel slides up, monospace code block with inline plain-English annotations, "Next Spell →" button

**Visual style:** Dark fantasy — deep purples/blues (consistent with existing dark palette), gold rune accents, soft glow on success. Monospace font for code, syntax highlighted in muted colors.

---

## Technical Architecture

**Single file:** `index.html` — HTML, CSS, JS all inline. No build step, no dependencies.

**State object:**
```js
{
  currentLevel: 0,      // index into LEVELS array
  slots: [null, null, null],  // player-placed runes
  solved: false         // true after correct cast
}
```

**Data — `LEVELS` array**, each entry:
```js
{
  objective: "Brew the invisibility potion...",
  recipe: ["fire", "water", "earth"],
  pool: ["fire", "water", "earth", "wind", "shadow"],  // recipe + distractors
  concept: "Arrays",
  tomeCode: `// A spell is just a list of steps\nconst spell = ["fire", "water", "earth"];\n// spell[0] is "fire", spell[1] is "water"...`
}
```

**Interaction:** HTML5 drag-and-drop API — runes dragged from pool into slots. Clicking a filled slot returns the rune to the pool.

**Win check:** On "Cast Spell", compare `slots[]` to `recipe[]` element-by-element. Match → success. No match → shake animation.

**Rendering:** DOM-based, re-rendered on state change. No canvas. CSS animations for success glow and tome slide-up.

---

## Out of Scope

- Sound effects (can be added later)
- Save/progress persistence
- Mobile touch support
- More than 8 levels in v1
