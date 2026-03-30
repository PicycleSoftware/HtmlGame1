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

### ⬜ 11. Ornament Parking Jam (`ornament-jam`)
**Concept:** Rush Hour / parking jam with Christmas ornaments. A grid of ornaments blocks the pickle ornament. Slide ornaments along rows and columns to clear a path and get the pickle to the exit.

**Mechanics:**
- Grid of ornaments (round, horizontal, vertical), each occupying 1–3 cells
- Ornaments slide freely along their axis until blocked — tap and drag to move
- Pickle ornament must reach the exit (edge of the grid)
- Seeded daily puzzle — same arrangement for everyone
- Score = number of moves (slides) used
- One-finger drag to slide; tap shows valid slide directions

**Why it's interesting:**
- Rush Hour is a proven, universally understood mechanic — zero learning curve
- Physical metaphor maps perfectly: ornaments hanging on branches, push them aside to reveal the hidden pickle
- Spatial reasoning without time pressure — ideal for all ages
- Satisfying "click" moment when the path clears and the pickle slides out

**Seed use:** Ornament positions, sizes, and orientations seeded. Pickle start position and exit seeded.

### ⬜ 12. Shake the Tree (`shake-tree`)
**Concept:** Swipe branches to shift ornaments; they slowly settle back. The pickle is buried under layered ornaments — only visible and tappable when you've cleared enough of the right area.

**Mechanics:**
- Tree-shaped play area packed with overlapping ornaments
- Swipe across a region to shift ornaments within it; they drift back to resting positions after a short delay
- Wide swipe displaces more ornaments but they settle back faster; precise swipe clears a smaller area but holds longer
- Pickle is hidden under layered ornaments — tap it to win when exposed
- Score = number of swipes used

**Why it's interesting:**
- Theme and mechanic are the same act — shaking things to find what's hidden is literally how you'd search a real Christmas tree
- Decision at every step: broad search vs. precise commit
- Tactile, physical feel; no gamer vocabulary required

**Seed use:** Pickle position and ornament layout seeded. Same shake sequence for everyone.

**Source:** Mechanic combination #103 (Area of Effect + Swipe to Move), rated A by Meier pass, Casual-Friendly by accessibility pass.

### ⬜ 13. Part the Branches (`part-branches`)
**Concept:** Drag vertical strips of tree branches left and right to open sightlines into the tree interior. The pickle is hidden inside — tap it when you've parted the right columns.

**Mechanics:**
- Tree rendered as layered vertical columns of overlapping branches
- Drag a column left or right to open a gap; adjacent columns partially block each other
- Pickle is in the interior — only tappable when a clear sightline exists through the right set of columns simultaneously
- Score = number of column drags used

**Why it's interesting:**
- Sightline logic maps naturally to peering into a dense tree
- Decision: which gap do you need, and what does creating it cost in adjacent columns?
- Physical metaphor is strong and requires no explanation

**Risk:** Underlying slider puzzle structure may trigger non-gamer resistance — validate with paper prototype before building.

**Seed use:** Pickle depth and position seeded. Column layout seeded.

**Source:** Mechanic combination #70 (Column Shifting + Drag to Slot), rated A by Meier pass, Casual-Friendly by accessibility pass.

### ⬜ 14. Pickle Hunt (`pickle-hunt`)
**Concept:** Spend ornament tokens to reveal hidden branches (fog of war), or save them — they also score points at game end. Same resource, two uses, every tap is a real trade.

**Mechanics:**
- Tree-shaped grid, all cells hidden under fog
- Player starts with a fixed supply of ornament tokens (seeded count)
- Spend a token: reveal a cell — shows either empty, ornament emoji, or the pickle (win)
- Tokens not spent on reveals score points at game end
- Score = tokens remaining when pickle is found (fewer reveals = better)

**Why it's interesting:**
- Reveal vs. save is immediately legible — no gamer vocabulary needed
- Scarcity gives the reveal moment weight; the last token spent on the right cell is a gasp
- Win moment: you gambled your last token and the pickle was there

**Source:** Mechanic combination #97 (Fog of War + Scarcity Management). Panel consensus: essential, strongest pick. *"Everything else should be compared against this." — Lazzaro*

### ⬜ 15. Pickle Garland (`pickle-garland`)
**Concept:** String ornaments into a scoring chain — each one added scores more than the last. But the next ornament is hidden; it might break the chain and score nothing. Stop and bank, or keep stringing?

**Mechanics:**
- Tree-shaped grid of hidden ornaments
- Player taps to add the next ornament to the garland; its type is revealed on tap
- Matching type extends the chain and multiplies score; wrong type breaks it (score nothing for that chain)
- Player can stop and bank the chain score at any time before tapping
- Pickle is hidden among the ornaments — finding it while stringing a chain triggers win + chain score bonus
- Score = total chain points banked

**Why it's interesting:**
- "Keep going or stop" is universally understood — Jenga, Blackjack, any childhood game
- Two win flavors: brave (extended and hit jackpot) or smart (banked at exactly the right moment)
- Even losing a chain is fun — you overreached

**Source:** Mechanic combination #195 (Hidden Information + Chain Scoring). Panel consensus: essential. *"Cleanest emotional signature of any mechanic on the list." — Lazzaro*

### ⬜ 16. Vault Trigger (`vault-trigger`)
**Concept:** Deduce where the pickle is hiding, then commit — step on the pressure plate that reveals a branch. The plate starts a process you can't undo. Are you sure enough?

**Mechanics:**
- Tree grid with hidden cells; clues scattered across revealed cells (hot/cold or directional)
- Player builds a deduction until they feel confident
- "Commit" tap on a cell triggers an irreversible reveal sequence (animation, no backing out)
- Wrong commit: reveal shows empty, costs extra moves, and the animation makes the failure memorable
- Right commit: pickle emerges with ceremony — the commitment payoff
- Score = moves used before correct commit

**Why it's interesting:**
- Commitment is the drama, not the deduction — thin logic layer, thick tension
- The irreversible moment is the held-breath mechanic; non-gamers understand "are you sure?"
- Win moment: you stepped on the plate and were right

**Source:** Mechanic combination #117 (Deduction + Pressure Plate). Panel consensus: essential.

### ⬜ 17. Press Your Branch (`press-branch`)
**Concept:** Each correct guess about the pickle's location adds a new constraint to the game. The puzzle tightens as you succeed. Push your luck — one more guess — or bank what you've narrowed down?

**Mechanics:**
- Tree grid; player makes directional guesses ("it's in the upper half", "it's on the left side")
- Each correct guess scores points AND adds a new rule constraining future guesses
- Each wrong guess costs moves
- As constraints accumulate the search space shrinks — but so does room for error
- Pickle is found when constraints narrow to a single cell
- Score = moves used

**Why it's interesting:**
- Jenga/Blackjack emotional DNA — "one more" is universally understood
- Game tightens as you succeed, building natural tension toward the end
- Win moment: the cascade of constraints collapses to one cell and the pickle is there

**Risk:** Constraint display must stay legible on a small screen — if rules pile up off-screen the session collapses.

**Source:** Mechanic combination #171 (Constraint Propagation + Pressing Luck). Panel consensus: strong.

### ⬜ 18. Branch or Deduce (`branch-deduce`)
**Concept:** Investigate where the pickle is hiding. You can deduce from existing clues (safe, slow) or open a speculative branch — committing to a hypothesis to force new clues to emerge. Branching costs a turn but unlocks information you couldn't get any other way.

**Mechanics:**
- Tree grid with hidden cells; some cells contain clues (revealed on tap)
- Each turn: either use a clue to eliminate cells (deduction, free) or "branch" — commit to a hypothesis zone, paying 1 move to reveal a clue cluster in that zone
- Wrong branch wastes the move; right branch shortcuts to the pickle
- Score = moves used before finding pickle

**Why it's interesting:**
- "Speculate to generate information" gives players agency over the deduction process
- Win moment feels personal — you took the branch nobody else would take and were right
- Serious Fun: *you* figured it out, not the system

**Source:** Mechanic combination #139 (Deduction Grid + Choice Branching). Panel consensus: strong (Brown's strongest miss). *"It means something that you figured it out." — Lazzaro*

---

## Maybe Someday

### Evidence Burn (`evidence-burn`)
**Concept:** Like Minesweeper but with only one special hidden cell (the pickle). Eliminate cells without revealing them for a small reward — but if you accidentally burn the pickle, it's gone. Reveal vs. eliminate, every turn.

**Why interesting:** Binary verb (reveal vs. eliminate) is clean; eliminating builds probability that the pickle is elsewhere, creating emergent deduction without a deduction system.

**Why it's a maybe:** Win moment is quiet — by the time you tap the last cell, the odds have already told you it's the pickle. Lazzaro: "diffuse win, doesn't feel like finding the pickle." Could work as a secondary mechanic layered onto another prototype rather than a standalone game.

**Source:** Mechanic combination #188 (Hidden Information + Binary Elimination).

### ⬜ 19. Pickle-sweeper (`pickle-sweeper`)
**Concept:** Minesweeper, but inverted — the numbered clues tell you how many pickle ornaments are in adjacent cells. You're hunting the pickles, not avoiding mines. Find all of them.

**Mechanics:**
- Tree-shaped grid, all cells hidden
- Tap to reveal a cell: shows a number (count of pickle ornaments in adjacent cells) or a pickle ornament (score!)
- No instant-death — revealing a pickle is the goal, not a failure
- Multiple pickles hidden per puzzle (seeded count and positions)
- Flag mode to mark suspected pickle cells
- Win = all pickles found
- Score = cells revealed before finding all pickles (fewer = better)

**Why it's interesting:**
- Zero learning curve for anyone who's seen Minesweeper; inverted goal removes the frustration of accidental death
- Familiar deduction loop with a friendlier emotional frame — you're collecting treasure, not tiptoeing through a minefield
- Simple enough to be one game in a multi-game daily format (Mario Party / WarioWare style)

**Seed use:** Pickle positions seeded. Same grid for everyone that day.

### ⬜ 20. House Exploration (`house-explore`)
Top-down RPG (Link's Awakening style). Walk through rooms of the house on Christmas Eve, talk to NPCs for clues, find the pickle hidden somewhere in the tree room.
- Grid-based movement, multiple rooms
- NPCs give directional or hot/cold clues
- Score = steps taken

### ⬜ 21. Metroidvania Tree (`metroidvania`)
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

## Mechanic Catalogue Process

A 3-part process to find new prototype ideas from well-known base mechanics. Run each part as a separate session.

### Part 1 — Catalogue (agents)
Run 2–3 research agents independently, each from a different design perspective:
- **Raph Koster** — game atoms, fundamental mechanic primitives
- **Zach Gage** — casual/mobile mechanics, accessible puzzle loops
- **Mark Brown** (GMTK) — puzzle mechanics, player verbs

Each agent produces a flat list of ~30–50 base mechanics: name, one-line description, canonical example game. **No Christmas Pickle filtering** — pure retrieval, no constraints applied.

Output file: `mechanic-catalogue.md`

### Part 2 — Combine (Chas + Claude)
Go through the catalogue together, deduplicate, and look for interesting combinations or stretches. This is the creative step — not delegated to agents. Flag anything worth exploring.

Output: a shortlist of combinations worth filtering.

### Part 3 — Filter (Chas + Claude, or agents)
Run the shortlist against the Christmas Pickle constraints:
- Turn-based, no time pressure
- One-finger, touch-friendly
- 2–5 minutes
- Ages 10–70, half non-gamers
- Daily fixed puzzle, shareable score

Decide what to add to the prototype queue.

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
