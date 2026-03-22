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

### ⬜ 3. String of Lights (`light-strings`) — TODO
Rotate tile segments to connect Christmas light strings. The pickle hides where all paths meet.
- Grid of rotatable pieces (straight, corner, T-junction) — tap to rotate 90°
- 2–3 strings must all connect
- Score = rotations used

### ⬜ 4. Branch Pusher (`branch-pusher`) — TODO
Sokoban-style: push ornaments and branches aside to reach the pickle buried underneath.
- Grid ~6×6
- Tap a block adjacent to player to push it
- Score = pushes used

### ✅ 6. Ornament Fling (`ornament-fling`) — DONE
Angry Birds-style drag and fling. Each fling reveals a zone of the tree.
- Tree silhouette divided into ~12 zones
- Drag back on ornament, release to fling at a zone
- Zones have different reveal costs (seeded)
- Score = flings used

---

## Build Order
1. ✅ Shared utilities + launcher shell
2. ✅ Tree Finder (validates daily seed + score system)
3. ✅ Tree Dungeon (validates navigation UX + reveal animation)
4. ⬜ String of Lights (validates puzzle/rotation UX)
5. ⬜ Branch Pusher
6. ✅ Ornament Fling

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
