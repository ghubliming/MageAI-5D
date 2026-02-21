---

## Plan (Stockfish-style + modern AI for 5D Chess)

This project will evolve from a single-timeline negaMax engine into a stronger 5D-capable engine by progressively adopting proven ideas from Stockfish and modern neural approaches.

### Phase 1 — “Stockfish-style” engine foundations
- **Clean engine architecture**
  - Separate: `movegen` / `legality` / `make-unmake` / `search` / `eval` / `time management`.
- **Iterative Deepening + aspiration windows**
  - Stable best-move reporting at any time limit; faster convergence.
- **Transposition Table (TT)**
  - Zobrist hashing; store best move, depth, bounds; critical for pruning and speed.
- **Stronger pruning & reductions**
  - Alpha-beta + PVS
  - Quiescence search (captures/checks)
  - Null-move pruning (where applicable/safe in 5D)
  - Late Move Reductions (LMR)
  - Futility pruning / razoring (careful with 5D tactics)
- **Move ordering upgrades**
  - History heuristic + continuation history
  - Improved killer usage
  - TT move first, then good captures (SEE), then quiets by history
- **Time management**
  - Basic clock handling + node budgeting; stop conditions.

### Phase 2 — 5D-specific correctness & scalability
- **Full multitimeline legality**
  - Make legality first-class (avoid “plays but illegal”).
- **State representation improvements**
  - Efficient timeline indexing and incremental updates for fast make/unmake.
- **5D-aware evaluation features**
  - Timeline safety and king safety across timelines
  - Threats that create/avoid “softmates”
  - Mobility that accounts for cross-timeline influence
  - Penalties for timeline overextension / unstable branches

### Phase 3 — “Modern AI” integration (NNUE / policy-value style)
- **NNUE-style evaluation (Stockfish-inspired)**
  - Lightweight neural eval that can be incrementally updated after moves.
  - Start with single-timeline training, then extend features to include timeline context.
- **Policy guidance (optional)**
  - Train a move-policy network to bias move ordering and reduce branching.
- **Self-play data pipeline**
  - Generate positions via engine self-play; store (position, best line/value, outcome).
  - Gradually shift from handcrafted eval to learned eval.

### Phase 4 — Strength and engineering
- **Testing + regression framework**
  - Perft tests (especially for 5D movegen/legality), tactic suites, ELO tracking.
- **Performance work**
  - Profiling + hot-path optimization; consider C++ port once rules/architecture stabilize.

---

# MageAI-5D
A 5D Chess AI utilizing [5d-chess-js](https://gitlab.com/alexbay218/5d-chess-js).

Currently the AI is a major work in progress and can only handle single timeline boards with standard pieces to about depth 6. In positions where checkmate is possible depth 15 is can be used to find checkmate

The static evaluation currently checks
1. Material
2. Movement per piece
3. King exposure
4. King Piece Defense

Currently I am using a negaMax search algorithm. 

The search optimizations are currently:
- Move ordering based on MVV-LVA
- Move ordering with checks
- Move ordering with Static Exchange Evaluation(SEE)
- Move ordering using killer heuristic
- A evaluation mode for detecting mates by returning 0 for states which are not checkmate.

WIP additions:
- Quiescence search
- Principal Variation Search(PVS)
- Iterative Deepening Depth First Search(IDDFS)
- Mate Distance Pruning
- History Heuristic

A full multitimeline AI will most likely not result from this for awhile and if it does happen will likely be written in C++. (Hopefully this AI will also be in C++ eventually)
A semi-working version that can play on mutliple timelines without considering legality though might be able to be implemented, but it will fail to actually refute softmates, and fail to consider cross timeline captures.

---

# MageAI-5D
A 5D Chess AI utilizing [5d-chess-js](https://gitlab.com/alexbay218/5d-chess-js).

Currently the AI is a major work in progress and can only handle single timeline boards with standard pieces to about depth 6. In positions where checkmate is possible depth 15 is can be used to find checkmate

The static evaluation currently checks
1. Material
2. Movement per piece
3. King exposure
4. King Piece Defense

Currently I am using a negaMax search algorithm. 

The search optimizations are currently:
- Move ordering based on MVV-LVA
- Move ordering with checks
- Move ordering with Static Exchange Evaluation(SEE)
- Move ordering using killer heuristic
- A evaluation mode for detecting mates by returning 0 for states which are not checkmate.

WIP additions:
- Quiescence search
- Principal Variation Search(PVS)
- Iterative Deepening Depth First Search(IDDFS)
- Mate Distance Pruning
- History Heuristic

A full multitimeline AI will most likely not result from this for awhile and if it does happen will likely be written in C++. (Hopefully this AI will also be in C++ eventually)
A semi-working version that can play on mutliple timelines without considering legality though might be able to be implemented, but it will fail to actually refute softmates, and fail to consider cross timeline captures.
