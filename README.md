# 🧩 Maze Generator

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Canvas](https://img.shields.io/badge/Canvas-FF6B6B?logo=canvas&logoColor=white)](#)
[![Algorithms](https://img.shields.io/badge/Algorithms-📚-green)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A procedural maze generator and pathfinding visualizer. Generate mazes using 5 classical algorithms, solve them with 5 pathfinding algorithms, and watch every step animated in real time on an HTML canvas — all from a single file with zero dependencies.

---

## ✨ Features

- **5 maze generation algorithms** — Recursive Backtracker, Prim's, Kruskal's, Eller's, Binary Tree
- **5 pathfinding algorithms** — A*, Dijkstra's, BFS, DFS, Greedy Best-First
- **Animated visualization** — watch the generation and solving unfold step by step
- **Side-by-side comparison** — compare two algorithms head-to-head on the same grid
- **Adjustable grid size** — from 5×5 tiny to 100×100 massive
- **Speed control** — from single-step frame advance to max-speed rendering
- **Complexity analysis overlay** — see time/space complexity per algorithm
- **Export maze as PNG image**

---

## 🏗️ Generation Algorithms

### Recursive Backtracker (DFS-based)

The gold standard. Uses a stack to perform randomized depth-first search, carving passages as it goes.

| Property | Value |
|----------|-------|
| Time Complexity | O(N) where N = rows × cols |
| Space Complexity | O(N) — stack in worst case |
| Maze Bias | Long corridors, few short dead ends |
| How it works | Pick a random unvisited neighbor, carve a path, push to stack. If stuck, backtrack. |

**Why it works**: It is essentially a randomized DFS traversal of a grid graph. Each cell is visited exactly once, so every cell is reachable — the maze is a **perfect maze** (exactly one path between any two cells).

### Prim's Algorithm

Grows the maze from a frontier. Maintains a set of frontier walls and randomly picks one to carve.

| Property | Value |
|----------|-------|
| Time Complexity | O(N log N) with a priority queue, O(N²) simple |
| Space Complexity | O(N) |
| Maze Bias | Short, twisty passages; many dead ends |
| How it works | Start from one cell, add its walls to frontier, pick random frontier wall that borders an unvisited cell, carve it, repeat. |

**Why it works**: This is Prim's minimum spanning tree algorithm applied to a grid where edge weights are random. The result is a spanning tree — a perfect maze.

### Kruskal's Algorithm

Treats each cell as its own set, then uses Union-Find to randomly connect components.

| Property | Value |
|----------|-------|
| Time Complexity | O(N · α(N)) — inverse Ackermann, practically linear |
| Space Complexity | O(N) |
| Maze Bias | Uniform distribution; no directional bias |
| How it works | Collect all walls. Shuffle randomly. For each wall: if the two cells are in different sets, carve and union them. |

**Why it works**: Like MST but edges are considered in random order. Union-Find with path compression makes set operations nearly O(1). Produces the most "balanced" mazes.

### Eller's Algorithm

Generates the maze **row by row**, using only O(width) memory. Ideal for infinite or extremely large mazes.

| Property | Value |
|----------|-------|
| Time Complexity | O(N) |
| Space Complexity | O(W) — only stores one row |
| Maze Bias | Slight horizontal bias |
| How it works | For each row, randomly join adjacent cells within the row (horizontal) and randomly connect cells to the next row (vertical), ensuring each set gets at least one vertical connection. |

**Why it works**: Sets ensure connectivity. The algorithm never needs to look at previous rows, making it memory-efficient for huge grids.

### Binary Tree

The simplest "maze" — each cell randomly connects either north or east.

| Property | Value |
|----------|-------|
| Time Complexity | O(N) — optimal |
| Space Complexity | O(1) beyond the grid itself |
| Maze Bias | Strong diagonal bias — a clear path along the diagonal |
| How it works | For each cell, flip a coin: carve north or carve east. Done. |

**Why it works**: Extremely fast but produces low-quality mazes with an obvious diagonal pattern. Good for generating the maximum number of dead ends. Often used as a teaching baseline.

---

## 🔍 Pathfinding Algorithms

### A* (A-Star)

The best of both worlds. Uses a heuristic to guide search toward the goal.

```
f(n) = g(n) + h(n)

g(n) = actual cost from start to node n
h(n) = heuristic estimate from n to goal (Manhattan distance)
```

| Property | Value |
|----------|-------|
| Time Complexity | O(E) worst case, typically much less |
| Space Complexity | O(V) — priority queue + visited set |
| Optimality | Yes — finds shortest path (with admissible heuristic) |
| How it works | Always expand the node with lowest f(n). Manhattan distance is admissible on grid mazes. |

### Dijkstra's Algorithm

A* without a heuristic — searches uniformly in all directions.

| Property | Value |
|----------|-------|
| Time Complexity | O((V + E) log V) with binary heap |
| Space Complexity | O(V) |
| Optimality | Yes — guarantees shortest path |
| How it works | Like A* but with h(n) = 0. Explores in concentric circles from the start. |

### BFS (Breadth-First Search)

Explores level by level — all nodes at distance d before distance d+1.

| Property | Value |
|----------|-------|
| Time Complexity | O(V + E) |
| Space Complexity | O(V) — the frontier queue |
| Optimality | Yes — shortest path in unweighted graphs |
| How it works | Uses a FIFO queue. Simple, predictable, but with no guidance toward the goal. |

### DFS (Depth-First Search)

Goes deep first, backtracking when stuck.

| Property | Value |
|----------|-------|
| Time Complexity | O(V + E) |
| Space Complexity | O(V) — recursion or stack |
| Optimality | **No** — can find long, winding paths |
| How it works | Uses a stack (LIFO). Dramatic when animated — aggressively pursues dead ends. |

### Greedy Best-First Search

Only uses the heuristic — ignores the cost so far.

```
f(n) = h(n)   (ignores g(n) entirely)
```

| Property | Value |
|----------|-------|
| Time Complexity | O(E) typical |
| Space Complexity | O(V) |
| Optimality | **No** — can get trapped |
| How it works | Always moves toward the point that *looks* closest to the goal. Fast but suboptimal. Great for contrast with A*. |

---

## 🎮 Controls

| Action | Control |
|--------|---------|
| Generate maze | Press `G` or click Generate |
| Solve maze | Press `S` or click Solve |
| Step animation | Press `→` arrow |
| Speed up | Press `+` |
| Slow down | Press `-` |
| Change generation algo | Press `1`-`5` |
| Change pathfinding algo | Press `Shift`+`1`-`5` |
| New random seed | Press `N` |
| Change grid size | Slider or `]` / `[` |
| Toggle comparison mode | Press `C` |
| Export as PNG | Press `X` |

---

## 🚀 Installation

```bash
git clone https://github.com/mayank-dev-15/maze-generator.git
cd maze-generator
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

Or serve locally:

```bash
python3 -m http.server 8080
# Visit http://localhost:8080
```

---

## 📖 Usage

1. **Pick a generation algorithm** — press `1`-`5` to switch. Each produces a distinctly different maze style.
2. **Generate** — press `G` and watch the algorithm carve passages in real time.
3. **Solve** — press `S` and pick a pathfinding algorithm. Watch it explore the maze.
4. **Compare** — press `C` for side-by-side mode. Run two algorithms on the same grid to compare.
5. **Experiment with size** — try a small 10×10 maze first, then scale up to 50×50 or 100×100.
6. **Export** — press `X` to download the current maze as a PNG.

---

## 📄 License

MIT License — free to use, modify, and distribute. See [LICENSE](LICENSE) for details.

---

*Algorithms on display. Learn by watching them work.* 🧠