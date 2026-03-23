# Claude Instructions — Christmas Pickle Game

## Project Plan
The active plan is at `PLAN.md` in this folder. Read it at the start of every session.

## Plan Maintenance
- After completing any prototype or meaningful step, update `PLAN.md` immediately:
  - Mark completed prototypes `✅ — DONE`
  - Mark removed/abandoned prototypes with a note (e.g. `❌ — Removed`)
  - Update the Build Order section to match
- If a prototype is redesigned mid-build, update its description in PLAN.md before continuing

## Project Rules
- All game code lives in a single file: `index.html` — no additional JS/CSS files
- No external dependencies, no server required — must open directly in any browser
- Every prototype uses the shared daily seed (`DAILY_SEED ^ unique_constant`) so puzzles differ per game but are consistent across devices on the same day
- Seed XOR constants must be valid JS hex literals (e.g. `0xDEADBEEF`) — do NOT use letters outside 0-9 A-F
- Tap targets must be ≥ 44px for mobile usability
- Test in Chrome DevTools iPhone SE emulator before marking a prototype done
