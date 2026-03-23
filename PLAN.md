# Christmas Pickle Game — Plan

## Concept
A Wordle-style daily puzzle game based on the Christmas Pickle tradition: a pickle-shaped ornament is hidden deep in the Christmas tree on Christmas Eve. The first person to find it on Christmas morning gets a special gift or good luck for the year.

**Core design goals:**
- Turn-based, no time pressure (pause and resume anytime)
- One-finger / touch-friendly (phone + desktop)
- Daily fixed puzzle — same seed for everyone that day, so family members can compare scores
- Score by efficiency: fewer moves = better
- All ages and skill levels (no quick reflexes required)
- Shareable score (like Wordle)

---

## File Structure
Single self-contained file — no server, no dependencies, opens directly in any browser.

```
HtmlGame1/
└── HtmlGame1/
    └── index.html   ← everything: HTML + CSS + JS
```

---

## Architecture

### Shared Utilities
- **Daily seed**: `YYYY-MM-DD` string → FNV-1a hash → Mulberry32 PRNG
- **Score storage**: `localStorage` keyed by `{date}-{prototypeId}`
- **Input**: unified `pointerdown/pointerup` handler mapping touch and mouse to canvas coordinates
- **Keyboard**: arrow keys / WASD wired up per game when active

### Launcher (Home Screen)
- Grid of prototype cards with name, description, today's score if played
- Show/hide `<div>` sections for navigation (no routing library)

### Prototype Interface
Each prototype is a JS class with:
```js
constructor(canvas, seed)   // build puzzle from seed
resize(w, h)                // refit to canvas size, re-render
handleTap(x, y)             // process a tap (canvas coords)
move(dr, dc)                // optional: directional movement
getControls()               // optional: returns button config array
renderCustomControls(bar)   // optional: fully custom controls HTML
onScoreUpdate(moves)        // callback — set by launcher
onWin(moves)                // callback — set by launcher
totalCells                  // property — used for score rating
```

---

## Prototypes

### ✅ 1. Tree Finder (`grid-reveal`) — DONE
Hot/cold grid reveal. Tap any hidden cell to reveal its graph distance to the pickle.
- Tree-shaped grid, 9×8, ~40 cells
- Distance labels: Very close / Closer / Far / Very far (color-coded)
- Flag mode to mark ruled-out cells
- Score = cells tapped before finding pickle

### ✅ 2. Tree Dungeon (`tree-dungeon`) — DONE
Roguelike exploration with fog of war and a slot machine reveal.
- ~1/3 of cells have a hidden ornament (✨ indicator visible from adjacent cell, but not what it is)
- Step into a cell with an item → slot machine reveal animates (fast → slow → lands on emoji with bounce)
- Pickle reveal triggers win after animation
- D-pad controls + tap-to-move + arrow keys
- Score = moves to reach pickle

### ✅ 3. String of Lights (`light-strings`) — DONE
Rotate tile segments to connect Christmas light strings. The pickle hides where all paths meet.
- 5×5 grid of rotatable pieces (straight, corner, T-junction, cross) — tap to rotate 90°
- 2 sources must connect to 1 sink; win = both sources reach sink via connected pipes
- Score = rotations used

### ✅ 4. Branch Pusher (`branch-pusher`) — DONE
Sokoban-style: push ornaments and branches aside to reach the pickle buried underneath.
- 6×6 grid, 9 pushable blocks, immovable trunk cells
- Tap a block adjacent to player to push it; walking is free, only pushes cost moves
- Pickle hidden under one block — revealed instantly when its block is pushed
- Score = pushes used

---

## Prototype 5 Candidates (pick one to build next)

### ✅ 5. The Pickle Moves (`pickle-chase`) 🏃 — DONE
**Concept:** Chase mechanic — the pickle migrates one cell per turn along a seeded random walk. It leaves a fading trail showing where it just was. Corner it before it escapes.

**Mechanics:**
- Same tree-shaped grid as Tree Finder (~40 cells)
- Pickle starts at a seeded position, moves to a random adjacent cell after each player turn
- Player taps any cell to "check" it — costs 1 move
- If pickle is there: win. If not: cell briefly shows whether the pickle was there recently
  - 🥒 (bright) = pickle was here last turn
  - 🥒 (faint) = pickle was here 2 turns ago
  - Empty = pickle hasn't been here recently
- Cells return to hidden after checking (old info goes stale)
- Score = checks used
- Keyboard: arrow keys move cursor, Enter checks cell

**Seed use:** Starting position + full movement sequence pre-seeded — same chase for everyone that day.

**Excitement factor:** Urgency and pursuit. You can see where it WAS but not where it IS. Commit and chase.

---

### ✅ 6. Press Your Luck (`press-luck`) 🎰 — DONE
**Concept:** Each cell has a visible seeded "cost" (1–3 moves). You also get a limited supply of hint tokens that eliminate a random non-pickle cell. Classic risk/reward gambling tension.

**Mechanics:**
- Same tree-shaped grid (~40 cells)
- Each cell shows its cost upfront: 🟢 = 1 move, 🟡 = 2 moves, 🔴 = 3 moves (seeded)
- Tapping a cell spends that many moves and reveals if the pickle is there
- Player starts with 3 hint tokens; each token crossed out one random non-pickle cell for free
- Score = total moves spent (lower = better)
- Keyboard: arrow keys navigate, Enter reveals, H uses a hint

**Seed use:** Cell costs seeded. Pickle position seeded. Hint targets seeded (same for everyone).

**Excitement factor:** Gambling tension. A 3-cost cell might be the pickle (jackpot!) or a 3-move waste. Hints are precious — spend them wisely.

---

### ✅ 7. Excavate (`excavate`) ⛏️ — DONE
**Concept:** Each cell has 1–3 hidden layers. Each tap digs one layer deeper and gives a distance hint. Deeper digs cost more but reveal stronger hints. The pickle is buried at the bottom layer of one cell — dig to find it.

**Mechanics:**
- Same tree-shaped grid (~40 cells)
- Each cell starts showing its depth as stacked lines (1, 2, or 3 layers) — seeded
- Tap once: dig layer 1 → reveals hot/cold hint (Very close / Closer / Far / Very far), costs 1 move
- Tap same cell again: dig layer 2 → tighter hint (Very close / Close only), costs 1 more move
- Tap again (if 3-layer cell): dig to bottom → reveals contents (ornament emoji or 🥒 pickle), costs 1 more move
- Pickle is always at the deepest layer of its cell — must dig all the way through to win
- Score = total taps used
- Keyboard: arrow keys move cursor, Enter digs

**Visual:** Cells show remaining depth as layered horizontal lines. Dug layers shown lighter/eroded.

**Seed use:** Cell depths seeded. Pickle cell and depth seeded.

**Excitement factor:** Slow excavation builds tension. You know you're getting closer on each layer but can't skip ahead. Efficient players probe shallow across many cells; bold players go deep fast.

### ⬜ 8. Nonogram / Picross (`nonogram`)
Fill in rows and columns to reveal the tree image. The pickle's position is encoded in the solution.
- Classic Picross rules: number clues on rows/columns indicate runs of filled cells
- Solved grid reveals a tree silhouette with the pickle cell highlighted
- Score = incorrect cells filled (errors)

### ⬜ 9. House Exploration (`house-explore`)
Top-down RPG (Link's Awakening style). Walk through rooms of the house on Christmas Eve, talk to NPCs for clues, find the pickle hidden somewhere in the tree room.
- Grid-based movement, multiple rooms
- NPCs give directional or hot/cold clues
- Score = steps taken

### ⬜ 10. Metroidvania Tree (`metroidvania`)
Side-scrolling tree with locked branches. Find key ornaments to unlock new sections and eventually reach the pickle.
- Left/right/up/down movement across branches
- Certain paths blocked until player finds matching key ornament
- Score = moves to reach pickle

---

## Game Component Ideas
Building blocks that could enhance or combine into prototypes — not strong enough as standalone games.

- **Pickle Reassembly** — pickle shattered and pieces are hidden around the tree. Finding each piece snaps it visibly onto a pickle outline. Good for: making progress feel cumulative and visible; could layer onto any exploration game.
- **Pickle Growth** — a pickle seed is planted somewhere; "watering" nearby zones shows organic growth as feedback. Good for: a more alive/tactile alternative to hot/cold distance labels.
- **Pickle Crafting** — ingredients (cucumber, dill, vinegar, salt) scattered across the tree/house, each behind a mini-puzzle. Combine to create the pickle. Good for: a ceremony/payoff moment; multi-stage structure.

---

## Considered and Discarded

| Idea | Reason discarded |
|---|---|
| **Mastermind** — guess ornament color sequence, get peg feedback | Boring |
| **Breakout** — bounce ball through tree layers | No real-time/reaction games |
| **Snake** — guide tinsel through grid | No real-time/reaction games |
| **Frogger** — hop across gifts to reach the tree | No real-time/reaction games |
| **Sliding Tile** — scrambled tree image, slide tiles to solve | Weak core loop; frustrating journey, satisfaction only at the end |
| **Fog-of-War Map** — reveal zones, get directional arrows | Weak core loop; tapping for info isn't a loop, overlaps with Tree Finder |

---

## Build Order
1. ✅ Shared utilities + launcher shell
2. ✅ Tree Finder (validates daily seed + score system)
3. ✅ Tree Dungeon (validates navigation UX + reveal animation)
4. ✅ String of Lights (validates puzzle/rotation UX)
5. ✅ Branch Pusher
6. ✅ Pickle Chase
7. ✅ Press Your Luck
8. ✅ Excavate

---

## Score Rating
| Moves / Total cells | Rating |
|---|---|
| ≤ 12% | ⭐⭐⭐ Incredible |
| ≤ 30% | ⭐⭐ Great |
| ≤ 55% | ⭐ Nice work |
| > 55% | 🔍 You found it! |

---

## Verification Checklist
- [ ] Opens directly in browser (no server)
- [ ] Works in Chrome DevTools mobile emulator (iPhone SE) — tap targets ≥ 44px
- [ ] Each prototype plays to completion and saves score to localStorage
- [ ] Changing the date (or overriding seed) produces a different puzzle
- [ ] Two different devices on the same date get the same puzzle layout
- [ ] Win screen share text copies correctly
