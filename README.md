
## Plan (Stockfish + Modern AI)

This project’s goal is to build a hybrid chess system that combines:
- **Stockfish** (a strong classical chess engine for calculation, evaluation, and tactical reliability)
- **Modern AI** (neural / learning-based components for style, long-term strategy, and adaptation)

### Phase 1 — Define the architecture
- Decide the main integration approach:
  - **AI-assisted Stockfish**: AI guides search parameters, move ordering, pruning, or selects among Stockfish candidate lines.
  - **Dual-engine voting**: Stockfish and AI both propose moves; a combiner chooses the final move.
  - **Teacher–student**: Stockfish generates high-quality labels/data; AI learns from it and later generalizes.
- Define clear module boundaries:
  - `engine/stockfish_adapter` (UCI, process management, analysis requests)
  - `ai/model` (inference + training)
  - `hybrid/combiner` (policy for picking final moves)
  - `data/` (games, positions, evaluations)

### Phase 2 — Integrate Stockfish reliably
- Add a clean UCI interface wrapper (start/stop engine, set options, analyze positions, parse output).
- Support:
  - FEN input/output
  - MultiPV (top-N candidate lines)
  - Time/Depth controls
- Write basic tests to ensure stable parsing and reproducible analysis.

### Phase 3 — Add the “Modern AI” component
- Start with inference-only (a baseline neural policy/value model).
- Define input encoding (board representation) and outputs:
  - policy: move probabilities
  - value: win/draw/loss or centipawn-like estimate
- Ensure the AI can evaluate a position quickly enough for practical use.

### Phase 4 — Hybrid decision logic
- Implement a combiner strategy, for example:
  - Let Stockfish generate top-N candidate moves.
  - Use AI to re-rank candidates (or break ties) based on value/policy.
  - Apply safety rules (avoid blunders by enforcing Stockfish tactical thresholds).
- Track metrics: strength (Elo via self-play), blunder rate, speed.

### Phase 5 — Training & iteration
- Data sources:
  - Self-play games
  - Human games
  - Stockfish-labelled positions (teacher data)
- Improve model via:
  - supervised learning (imitate strong play)
  - reinforcement learning (self-play fine-tuning)
- Evaluate regularly with automated test suites and match results.

### Phase 6 — Packaging & usability
- Provide a simple CLI and/or UI:
  - analyze a position
  - play a game vs. the hybrid engine
  - export PGNs
- Document how to run everything locally and how to configure Stockfish + AI.



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
