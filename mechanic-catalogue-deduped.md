# Mechanic Catalogue — Unified Deduplicated List

*Distilled from three independent source catalogues: Catalogue A (Raph Koster lens), Catalogue B (Zach Gage lens), Catalogue C (Mark Brown / GMTK lens). Three deduplication passes applied: Koster (structural merge), Brown/GMTK (grain-size refinement), Lazzaro (emotional/social gap check). No new mechanics invented — all entries originate in the source catalogues or are documented real-game mechanics flagged as genuine blindspots.*

*~127 distinct mechanics across 16 categories.*

---

## Spatial / Movement

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Pathfinding / Navigation** | Player moves a token or avatar through a defined space toward a goal | Pac-Man |
| **Chase & Flee** | One agent pursues another; the pursued agent evades | Tag (folk game) |
| **Territory Control** | Players claim and defend regions of a shared space; ownership persists | Go |
| **Zone Control** | Threatening or occupying cells restricts opponent movement options without necessarily claiming ownership | Chess |
| **Area Enclosure** | Player draws a boundary around empty space to claim it; the verb is surrounding rather than occupying | Go (territory scoring) |
| **Sliding Block** (aka Tile Sliding) | Player shifts constrained pieces within a closed grid to achieve a configuration | 15-Puzzle |
| **Push Block** | Player moves an object by walking into it; object slides until it hits an obstacle | Sokoban |
| **Pull Block** | Player drags an object toward themselves as they move away from it | Baba Is You |
| **Grid Movement** | All entities occupy discrete cells; actions resolve one step at a time | Into the Breach |
| **Momentum / Sliding** | Entity continues moving in its current direction until it hits an obstacle | Pokémon puzzle games |
| **Placement** | Player positions a piece on a board where position relative to existing pieces determines value | Othello / Reversi |
| **Tile Placement** | Player places a shaped tile onto a grid without overlapping existing pieces | Tetris / Blokus |
| **Stacking** (aka Object Stacking) | Player physically or logically stacks objects; stability or height is the goal | Jenga |
| **Maze Traversal** | Player navigates a branching space with dead ends, seeking an exit | Labyrinth |
| **Collision Avoidance** | Player steers an avatar to avoid oncoming objects or enemies | Asteroids |
| **Teleportation** | Player or object jumps to a paired or targeted location | Portal |
| **Rotation** (aka Tile Rotation) | Object or player rotates in place, changing its facing, footprint, or edge connections | Tetris / Carcassonne |
| **Path Following** | Entity moves along a fixed or player-defined route | Lemmings |
| **Wrapping Space** | Edges of the map connect to opposite edges | Pac-Man |
| **Layered Planes** | Multiple overlapping grids occupy the same nominal space | Manifold Garden |
| **Scale Change** | Player or object changes size, altering what it can pass through or interact with | Superliminal |
| **Mirror / Symmetry** | An action simultaneously produces its reflection in another region | PuzzleScript mirror levels |
| **Gravity Shifting** (aka Gravity Direction) | Player flips or redirects the gravity axis to navigate a space | VVVVVV |
| **Path Drawing** | Player draws a continuous line connecting two matching endpoints through a grid | Flow Free |

---

## Dexterity / Timing

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Rhythm Matching** | Player inputs commands in sync with a beat or visual cue | Dance Dance Revolution |
| **Aim & Shoot** | Player targets a moving or static object and fires a projectile | Space Invaders |
| **Catch / Intercept** | Player positions to intercept a moving object before it passes | Pong |
| **Timed Gate** | Player acts within a narrow time window to succeed | Guitar Hero |
| **Balancing** | Player makes micro-corrections to keep a system in equilibrium | Keep Talking and Nobody Explodes |
| **Launch & Trajectory** (aka Projectile Aiming) | Player sets angle and power; physics carry the outcome to a target | Angry Birds |
| **Ball Bouncing** | Player aims a ball that deflects off surfaces toward targets | Breakout |
| **Slice Cutting** | Player swipes to cut objects cleanly along a traced line | Fruit Ninja |

---

## Pattern Recognition / Information

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Memory Match** (aka Card Flipping, Memory Tiles) | Player recalls and pairs previously revealed items after they are re-hidden | Concentration |
| **Pattern Completion** | Player identifies the rule in a partial sequence or shape and extends or closes it | Tetris / IQ puzzles |
| **Deduction** | Player narrows possibilities by eliminating contradictions from clues | Clue / Cluedo |
| **Hidden Information** | System conceals game state; players act on incomplete knowledge | Poker / Clue |
| **Fog of War** | Areas not currently in view are hidden or only partially remembered on the map | Civilization |
| **Inference from Action** | Player deduces an opponent's hidden state by observing what moves they chose to make | Battleship / Stratego |
| **Signal Detection** | Player distinguishes meaningful signal from noise or decoys in a set | Zendo |
| **Sorting / Classification** | Player categorizes objects by recognizing shared attributes | Spot It! (Dobble) |
| **Tile Matching** | Player removes tiles that share a property (color, symbol) from a board | Mahjong Solitaire |
| **Witness Testimony** | Clues sourced from entities at a scene; player infers truth from partial or conflicting accounts | Return of the Obra Dinn |
| **Line of Sight** | Entity can only perceive or act on targets in an unobstructed line | Into the Breach |

---

## Word & Letter

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Letter Selection** | Tap individual letters to build a word from a fixed set | Letterpress |
| **Letter Chaining** | Drag through adjacent letters to form a word | Boggle / SpellTower |
| **Letter Placement** | Place a letter tile onto a grid position | Scrabble |
| **Column Shifting** | Slide columns of letters up or down to align valid words across rows | Typeshift |
| **Word Guessing** | Submit a word; system reveals positional correctness of each letter | Wordle |
| **Letter Swap** | Exchange one letter for another to fix or build a word | Jumble |
| **Anagram Solving** | Rearrange a scrambled set of letters into valid words | Anagram puzzles |
| **Prefix / Suffix Chaining** | Extend a word by adding valid letter groups to either end | Bananagrams |

---

## Card

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Card Stacking** | Place a card onto another by a matching rule (suit, rank, or color) | Klondike Solitaire |
| **Card Sequencing** | Arrange cards into ordered runs | FreeCell |
| **Hand Management** | Choose which cards to play, hold, or discard from a limited hand | Slay the Spire |
| **Trick Taking** | Play a card to beat or follow suit against opponents | Spades |
| **Drafting** (aka Card Drafting) | Players take turns selecting from a shared visible pool, depleting it as they go | 7 Wonders |
| **Column Building** | Stack cards into columns with specific positional rules | Flipflop Solitaire |
| **Deck Cycling** | Draw through a fixed deck, replaying it to find a usable card | Klondike |
| **Pile Matching** | Play a card that matches the top of any open pile | Uno |
| **Card Capture** | Take cards from the table by matching or summing to a target value | Casino |
| **Deck / Pool Building** | Player constructs and refines their own decision-space over the course of play | Dominion |
| **Tableau Building** | Player builds a personal display of cards or tiles that generates ongoing passive effects | 7 Wonders / Wingspan |

---

## Logic & Deduction

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Constraint Satisfaction** (aka Number Placement, Satisfying All Goals) | Assign values or place elements so all stated rules are simultaneously satisfied | Sudoku / Sokoban |
| **Constraint Propagation** | Placing one element forces the values of others by logical elimination | Sudoku |
| **Hidden Information Deduction** | Use revealed clues to infer the state of hidden cells in a grid | Minesweeper |
| **Binary Elimination** | Possibilities are systematically ruled out by contradiction until only one remains | Logic grid puzzles / Obra Dinn |
| **Deduction Grid** (aka Region Marking) | Player eliminates possibilities using a matrix of known facts, flagging cells as in or out | Nonogram / Picross |
| **Forced Move Detection** | Some positions have only one legal or non-losing continuation | Chess endgame studies |

---

## Economic / Resource

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Resource Accumulation** | Player collects fungible units over time to spend later | Monopoly |
| **Conversion / Crafting** | Player transforms input resources into different output resources | Settlers of Catan |
| **Auction / Bidding** | Players compete by committing resources; highest bid wins | Ra (Knizia) |
| **Trading** | Players voluntarily exchange resources to mutual or asymmetric benefit | Pit |
| **Scarcity Management** | Player manages a resource that depletes and cannot be fully replenished | FTL |
| **Investment & Return** | Player spends now to receive a multiplied payoff later | Agricola |
| **Pressing Luck** | Player decides when to stop accumulating before a risk event wipes the gain | Blackjack / Can't Stop |
| **Probability Estimation** | Player explicitly assesses odds to choose between options; the skill is reading the distribution | Poker / Settlers of Catan |
| **Limited Uses** | An action or tool can only be performed a fixed number of times before it is gone | Portal (early) |
| **Consumption** | Using an item or space depletes it permanently and irrecoverably | Minesweeper flags |
| **Action Points** | Each turn a unit has a fixed budget to spend across sub-actions | XCOM |
| **Worker Placement** | Player commits a token to a location to claim exclusive access to an action for a round | Agricola / Lords of Waterdeep |
| **Cooldown** | An action becomes available again only after a set delay | Slay the Spire |
| **Area of Effect** | An action resolves across a zone rather than a single target | Into the Breach |

---

## Social / Negotiation

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Bluffing** | Player misrepresents game state or intention; others decide to believe or call | Liar's Dice |
| **Constrained Communication** | Players may only communicate through a limited vocabulary or channel | Codenames / Dixit |
| **Voting / Consensus** | Players collectively decide an outcome through vote or majority rule | Werewolf / Mafia |
| **Alliance Formation** | Players form temporary coalitions that shift as the power balance changes | Diplomacy |
| **Role Deduction** | Players attempt to identify hidden roles others have been assigned | Among Us |
| **Asymmetric Roles** | Players operate under different rule sets and win conditions within the same game | Netrunner / Root |
| **Promises & Betrayal** | Players make binding or non-binding commitments and choose whether to honor them | Diplomacy |

---

## Combinatorial / Puzzle

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Set Collection** | Player gathers specific combinations of items that score together | Rummy / Set |
| **Connection / Network** | Player links nodes to form unbroken chains or networks for scoring | Ticket to Ride |
| **Dot Connecting** | Player connects dots in sequence to close a shape or claim a region | Dots and Boxes |
| **Elimination** (aka Capture / Elimination) | Player removes opponent pieces by capturing, blocking, or outscoring | Chess |
| **Blocking / Obstruction** | One entity physically prevents another from passing or acting (without removing it) | Rush Hour |
| **Key and Lock** | A specific item or action enables access to a blocked resource or area | Zelda dungeons |
| **Chain Scoring** | Player selects a sequence of matching items in one continuous action for bonus points | Candy Crush |
| **Color Sorting** | Move colored items into discrete groups via restricted intermediate moves | Color Sort (tube puzzle) |
| **Stack Sorting** | Reorder a stack by moving items through a limited number of staging areas | Tower of Hanoi |

---

## Causality & Chain Reactions

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Pressure Plate** | Continuous occupancy of a trigger activates an effect for as long as it is held | Portal |
| **Domino Chain** | One action triggers a sequence of dependent events that resolve automatically | SpaceChem |
| **Lever Toggle** | Player flips a binary state that persists until flipped again | The Witness |
| **Conditional Rule** | An effect applies only when a stated condition is true | Baba Is You |
| **Displacement Cascade** | Moving one piece forces downstream pieces to move, creating positional chain reactions | Rush Hour |
| **Tethering** | Two entities are linked; movement of one constrains or pulls the other | Stephen's Sausage Roll |

---

## Rule Manipulation

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Rule Rewriting** | Player changes the laws governing entity behavior mid-game | Baba Is You |
| **Role Swap** | Player or entity exchanges properties or roles with another entity | Baba Is You |
| **Constraint Inversion** | What was an obstacle becomes an enabler; a blocking condition now opens access | Antichamber |

---

## Time & Order

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Undo** | Player reverses the last action or sequence of actions without penalty | Stephen's Sausage Roll |
| **Simultaneous Resolution** | All entities act at once; player must predict and plan around simultaneous collisions | Into the Breach |
| **Turn Order** | Entities act in a fixed or variable sequence that can be tracked or manipulated | Invisible Inc. |
| **Time Rewind** | Player replays a window of past time, optionally taking different actions | Braid |
| **Sequence Locking** | Earlier actions irrevocably constrain what later actions are possible | Sokoban |
| **Race to Threshold** | First player to reach a numeric target or finish line wins | Snakes and Ladders |
| **Catch-Up Mechanic** | System disadvantages the leader to compress outcomes and maintain tension | Mario Kart |

---

## Progression & Unlocking

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Skill Tree / Ability Unlocking** | Completing milestones gates access to new verbs or options; action space expands over time | Diablo / Final Fantasy |
| **Technology Tree** | Researching nodes on a directed graph unlocks descendant nodes; earlier choices constrain later option sets | Civilization |

---

## Narrative / Simulation

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Choice Branching** | Player selects from options that alter the narrative or state in lasting ways | Zork |
| **Simulation Management** | Player adjusts parameters of a running system to keep it within a target state | SimCity |
| **Role Embodiment** | Player adopts a persistent identity with stats and makes decisions in character | Dungeons & Dragons |
| **Emergent Storytelling** | System generates events; player interprets and responds to them as narrative | Dwarf Fortress |

---

## Unit Interaction

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Copying / Cloning** | Player actions are replicated by a second agent simultaneously or with a time delay | The Swapper |
| **Object Dragging** | Player picks up and repositions an object to satisfy a spatial constraint | Monument Valley |

---

## Touch-Native Interaction Patterns

| Name | Description | Canonical Example |
|------|-------------|-------------------|
| **Swipe to Move** | Directional swipe moves all pieces in that direction simultaneously | 2048 / Threes |
| **Tap to Cycle** | Tap an object to advance it through a fixed set of states | Pipe Dream |
| **Long Press Reveal** | Hold on an item to surface hidden information or options | Mobile puzzle UIs broadly |
| **Drag to Slot** | Drag an item from one zone and drop it into a valid target slot | Card / tile games broadly |
| **Pinch to Zoom** | Pinch gesture changes scale, revealing structure at different levels | Gorogoa |

---

## Blindspots

*Mechanic types present in real games that were absent from all three source catalogues. Flagged by the Lazzaro pass (emotional/social lens). Not included in the main catalogue above — requires human review before use.*

| Gap | Lazzaro Key | Example Games | Why Distinct |
|-----|-------------|---------------|--------------|
| **Incremental / Idle Progression** | Serious Fun | Cookie Clicker, A Dark Room | Not active-choice accumulation; emotion is ambient, low-arousal return-reward |
| **Permadeath / Irreversible Loss** | Serious Fun | FTL, Hades, Nethack | Consumption covers item depletion; this is run-level permanent erasure of investment |
| **Calibrated Difficulty / Adaptive Challenge** | Serious Fun | RE4, Left 4 Dead Director | Catch-Up Mechanic is player-to-player compression; this is system-to-player flow maintenance |
| **Shared Fate / Cooperative Win Condition** | People Fun | Pandemic, Hanabi | Alliance Formation allows defection; cooperative structure has no defection axis |
| **Audience / Spectator Participation** | People Fun | Jackbox, Keep Talking | No existing mechanic addresses observer-as-active-participant |
| **Social Deduction via Live Performance** | People Fun | Werewolf, Secret Hitler | Role Deduction is inference; this is expressive physical performance that others read |
| **Collective Building / Collaborative Construction** | People Fun | Minecraft, Overcooked | Worker Placement and Tableau Building are personal engines, not joint authorship |
| **Environmental Reactivity / Toybox Interaction** | Easy Fun | Katamari, Goat Simulator | Domino Chain requires a solution goal; this is curiosity-driven with no correct outcome |
| **Reveal / Unmasking** | Easy Fun | Pictionary, Dixit, Mysterium | Deduction mechanics name the reasoning process; this names the instantaneous disclosure moment as its own beat |

---

*Part 1 complete. Proceed to Part 2 (combine + shortlist) when ready.*
