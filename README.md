# 🧠 Dynamic Wumpus Logic Agent

An intelligent agent built to navigate the treacherous Wumpus World, using **propositional logic and resolution-based inference** to avoid pits, evade the Wumpus, and retrieve gold. The agent learns in real time, maintains a knowledge base (KB), and proves safety before moving.

## 🎯 Objective
The agent starts in the top-left corner (0,0) and must:
1. **Find the gold** without stepping into a pit or the Wumpus.
2. **Avoid hazards**:
   - **🕳️ Pits** → instant death (`Agent fell into a pit!`)
   - **🐉 Wumpus** → instant death (`Agent was killed by the Wumpus!`)
3. **Use sensors**:
   - **🌊 Breeze** → adjacent to a pit.
   - **💀 Stench** → adjacent to the Wumpus.
   - **✨ Glitter** → gold is in the current cell.

## 🧠 Intelligent Agent Design

### 🔍 Knowledge Base (KB)
- The agent maintains a **logical KB** using clauses about pits (`P`) and Wumpus (`W`).
- Each visited cell adds facts:
  - If **no breeze** → all adjacent cells are *pit-free*.
  - If **breeze** → at least one adjacent cell contains a pit.
  - Same for stench/Wumpus.

### ⚡ Resolution-Based Inference
- Before moving into an **unknown** cell, the agent tries to prove it is **safe** using resolution refutation.
- If `¬P(x,y)` and `¬W(x,y)` can be derived from the KB, the cell is marked **safe**.
- Inference count tracks the number of resolution steps performed.

### 🤖 Automated Decision Making
- **Frontier expansion**: The agent prioritizes:
  1. **Known safe & unvisited** cells.
  2. **Frontier cells** (adjacent to visited, but not proven safe).
  3. **Unknown cells** (fallback).
- **Pathfinding**: BFS to the nearest target cell.
- **Auto mode**: Continuous stepping with 480ms interval.

## 🕹️ Features

| Control | Action |
|---------|--------|
| **NEW HUNT** | Generate random world & restart agent. |
| **AUTO HUNT** | Agent moves autonomously using logic. |
| **STEP** | Single logical step (manual mode). |
| **REVEAL** | Toggle map hack – shows pits/Wumpus. |
| **Grid click** | Manually move agent to adjacent cell (only if safe). |

### 🧪 World Generation
- Grid size: **3×3 up to 7×7** (adjustable).
- Pits: ~15% of cells.
- Wumpus: 1 random cell (not start).
- Gold: 1 random cell (not start).
- Sensors (breeze/stench) are computed automatically based on neighbors.

### 📊 Metrics Panel
- **Moves** – number of steps taken.
- **Safe** – cells proven safe via resolution.
- **Resolve** – total inference steps performed.
- **Frontier** – unvisited, unproven but adjacent to visited.
- **Unknown** – completely unexplored cells.
- **Visited** – cells entered by the agent.

## 🧩 Resolution Engine Details

The agent uses a **simple but functional resolution theorem prover**:
- Clauses are built from visited cells' sensor data.
- To prove `P(x,y)` or `W(x,y)`, the negated literal is added to the KB.
- If an empty clause is derived → the original literal is **true** (unsafe).
- If resolution exhausts without empty clause → literal is **false** (safe).

> ✅ This allows the agent to deduce safety even without exploring every cell.

## 🎮 Game States

| State | Visual | Behavior |
|-------|--------|----------|
| Hunting | `status-active` | Normal sensing & moving. |
| Gold Found (pending) | `status-victory-pending` | Waits 2 seconds, then auto-victory. |
| Victory | `status-victory` | Gold collected – stops agent. |
| Death (pit) | `status-dead` | shows *Agent fell into a pit!* |
| Death (Wumpus) | `status-dead` | shows *Agent was killed by the Wumpus!* |

## 🧱 Technical Implementation

- **Pure HTML/CSS/JS** – no external dependencies.
- **Grid rendering** – dynamic size & color-coded cells.
- **Responsive layout** – sensors on the left, grid on the right.
- **Real-time updates** – move count, inference count, safety stats.

## 🚀 How to Run

1. Save the provided HTML file as `wumpus.html`.
2. Open in any modern browser (Chrome, Firefox, Edge).
3. Click **NEW HUNT** to generate a random world.
4. Click **AUTO HUNT** to watch the logic agent work.
5. Or click cells manually to move step-by-step.

> 🔒 No build steps, no server – runs entirely client-side.

## 📈 Example Inference

If the agent visits a cell with:
- **Breeze = false** and **Stench = false**
- It adds `¬P(neighbor)` and `¬W(neighbor)` for all four adjacent cells.
- Those neighbors become **safe** immediately without visiting.
- The agent can plan a path through them to explore further.


## 👨‍💻 Author

Created as a demonstration of **logical reasoning in a dynamic environment** – combines classic AI (Wumpus World) with modern UI and real-time resolution.

---

### 📝 License

Feel free to use, modify, and distribute for educational purposes.
