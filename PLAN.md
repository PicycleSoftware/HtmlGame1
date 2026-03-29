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

### ✅ 8. Nonogram / Picross (`nonogram`) + v2 idea — DONE
Fill in rows and columns to reveal the tree image. The pickle's position is encoded in the solution.
- Classic Picross rules: number clues on rows/columns indicate runs of filled cells
- Solved grid reveals a tree silhouette with the pickle cell highlighted
- Score = incorrect cells filled (errors)

### ✅ 9. Tree Dungeon 2 (`tree-dungeon-2`) — DONE
Same fog-of-war dungeon as Tree Dungeon, but ✨ cells now hide either a hot/cold distance clue (🔍) or the pickle (🥒).
- Slot machine cycles 🥒/🔍, lands on the truth
- Clue reveal shows distance label and color (Very close / Closer / Far / Very far) — same as Tree Finder
- Pickle reveal runs the slot machine 2× longer before landing
- Score = moves to reach the pickle

### ✅ 10. Tree Finder 2 (`tree-finder-2`) — DONE
Same hot/cold grid as Tree Finder with hint presents under the tree.
- Same 9×8 tree-shaped grid (~40 cells)
- 3 gift presents below the tree, each different colour (red / gold / blue)
- Opening a present reveals a directional hint via slot-machine animation (window grows as symbols spin, decelerates to result with bounce)
- Present costs: 2 / 4 / 6 moves (escalating — opening a present spends moves)
- Hint types: Left vs. Right half · Upper vs. Lower half · Branch tips vs. Near center
- Strategy: open presents for guaranteed intel vs. spend moves searching directly
- No flag mode (cleaner UX; presents replace the intel-gathering role)
- Score = total moves (cell taps + present costs)

### ⬜ 10. Lottery Scratcher (`scratcher`)
**Concept:** Classic lottery scratcher on the Christmas tree. Scratch cells to reveal what's underneath — some are instant prizes, some give clues, one hides the pickle.

**Mechanics:**
- Tree-shaped grid, same shape as Tree Finder
- Each hidden cell has a seeded type: empty, prize, clue, or pickle
- Tap/scratch to reveal — each scratch costs 1 move
- Prize cells give something useful: +3 moves back, a directional clue, or a free reveal of a nearby cell
- Clue cells show a distance label (like Tree Finder) — cheaper intelligence
- Pickle cell triggers win with slot machine celebration
- Matching mechanic idea: find 2-of-a-kind or 3-of-a-kind of certain ornament symbols to earn a bonus reward
- Score = scratches used before finding pickle (prize refunds count)

**Why it's interesting:**
- Each scratch has variable value — resource management between safe info-gathering (clue cells) and gambling for prizes
- The matching mechanic adds a secondary goal that rewards thorough exploration
- Slot machine reveal vocabulary (already proven in Tree Dungeon) fits lottery aesthetic perfectly
- Prize economy creates decisions: use the bonus now or bank it?

**Seed use:** All cell types seeded. Matching symbols seeded. Prize values seeded.

### ⬜ 11. House Exploration (`house-explore`)
Top-down RPG (Link's Awakening style). Walk through rooms of the house on Christmas Eve, talk to NPCs for clues, find the pickle hidden somewhere in the tree room.
- Grid-based movement, multiple rooms
- NPCs give directional or hot/cold clues
- Score = steps taken

### ⬜ 12. Metroidvania Tree (`metroidvania`)
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
8. ✅ Nonogram (Pickle-o-gram)
9. ✅ Excavate
10. ✅ Tree Dungeon 2
11. ✅ Tree Finder 2

---

## Score Rating
| Moves / Total cells | Rating |
|---|---|
| ≤ 12% | ⭐⭐⭐ Incredible |
| ≤ 30% | ⭐⭐ Great |
| ≤ 55% | ⭐ Nice work |
| > 55% | 🔍 You found it! |

---

## Versioning

The launcher title displays a version number (e.g. `v0.11`). Increment it with every commit that changes game code or behavior. The version lives in `index.html` in the `<h1>` on the launcher screen.

---

## Verification Checklist
- [ ] Opens directly in browser (no server)
- [ ] Works in Chrome DevTools mobile emulator (iPhone SE) — tap targets ≥ 44px
- [ ] Each prototype plays to completion and saves score to localStorage
- [ ] Changing the date (or overriding seed) produces a different puzzle
- [ ] Two different devices on the same date get the same puzzle layout
- [ ] Win screen share text copies correctly
