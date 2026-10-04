# IAI-SLE3-25UAM032
# 8-Puzzle Solver: Architectural Design & Performance Profiling (BFS vs. DFS)

This repository contains the algorithmic implementation, benchmark profiling, and complete **C4 Architectural Model** documentation for the 8-puzzle problem using **Breadth-First Search (BFS)** and **Depth-First Search (DFS)**[cite: 10, 11].

* **Course:** 02AML204 – Introduction to Artificial Intelligence[cite: 10, 11]
* **Module:** SLE-3 Architectural Design (Full C4 Model)[cite: 10, 11]
* **System Focus:** 3×3 8-Puzzle Solver using Uninformed Search Algorithms[cite: 10, 11]

---

## 📌 1. System Overview

The 8-puzzle is a classical AI state-space search problem modeled on a 3×3 grid containing tiles numbered 1 to 8 and a blank space represented as `0`[cite: 10, 11]. The primary goal is to discover a path of transitions from an initial configuration to a predefined goal state[cite: 11]:

* **Initial State:** `(1, 2, 3, 0, 4, 6, 7, 5, 8)`
* **Goal State:** `(1, 2, 3, 4, 5, 6, 7, 8, 0)`
* **Optimal Solution Path Length:** 5 moves

The system implements graph-search adaptations of both algorithms by maintaining explored/visited state registries to prevent state-space cycles and infinite looping[cite: 10, 11].

---

## 🏗 2. C4 Architecture Model Overview

Following the C4 architecture framework (Context, Container, Component, Code), the system structure spans high-level system context down to core execution routines[cite: 10, 11]:

### Level 1: Context Diagram
* **User / Operator:** Supplies the starting puzzle state and requests solution execution[cite: 10, 11].
* **8-Puzzle Solver System:** The central search application that coordinates state expansions, goal testing, and search strategies[cite: 10, 11].
* **Performance & Search Output:** Outputs path optimality, total expanded state count, execution times, and visual profiling flamegraphs[cite: 10, 11].

### Level 2: Container Diagram
The application is structured into five logical functional units[cite: 10, 11]:
1. **Input / Configuration Container:** Encapsulates constants (`START_STATE`, `GOAL_STATE`, `MOVES`)[cite: 11].
2. **State Manager:** Manages tuple immutability and generates legal successor transitions (`get_neighbors`)[cite: 10, 11].
3. **Search Engine:** Orchestrates traversal mechanics using FIFO queues for BFS and LIFO stacks for DFS[cite: 10, 11].
4. **Visited State Management:** Hash-set lookup structures preventing redundant state exploration and cyclic traps[cite: 10, 11].
5. **Output & Profiling Module:** Calculates millisecond latencies via `timeit` and captures CPU call stacks via `py-spy`[cite: 10, 11].

### Level 3: Component Diagram (Search Engine Container)
Drilling into the **Search Engine Container** reveals five core operational blocks[cite: 10, 11]:
* **BFS Search Routine:** Executes level-by-level exploration using a double-ended queue (`collections.deque`)[cite: 11].
* **DFS Search Routine:** Executes branch exploration using a stack list with a fixed cutoff depth ($d=15$)[cite: 11].
* **Frontier Management:** Coordinates push and pop operations across the respective data structures[cite: 11].
* **Goal Test Unit:** Evaluates whether candidate successor states match the target board layout[cite: 10, 11].
* **Successor Generator:** Calculates blank tile row-column offsets and translates board transformations[cite: 10, 11].

### Level 4: Code-Level Overview
Core functions and data structures defining system behavior[cite: 10, 11]:
* `get_neighbors(state)`: Identifies `0` index via `divmod` and yields valid directional successor boards[cite: 11].
* `bfs(start, goal)`: Executes queue-based level-order search, guaranteeing shortest-path discovery[cite: 11].
* `dfs(start, goal, max_depth=15)`: Executes depth-bounded stack-based search[cite: 11].
* `compare_search_methods()` / `timeit` suite: Performs automated benchmarking across execution runs[cite: 11].

---

## 📊 3. Empirical Performance Comparison

Benchmarked using Python's `timeit` framework over test iterations:

| Metric | Breadth-First Search (BFS) | Depth-First Search (DFS) | Architectural Finding |
| :--- | :--- | :--- | :--- |
| **Average Execution Time** | ~2.152 ms | ~32.418 ms | BFS is ~15× faster on shallow goals[cite: 11] |
| **Nodes Expanded** | 74 nodes | 1280 nodes | DFS explores deep branches before backtracking[cite: 11] |
| **Memory Footprint** | $\mathcal{O}(b^d)$ (Frontier queue) | $\mathcal{O}(b \cdot m)$ (Stack depth) | DFS frontier stays lean; BFS stores all frontier nodes |
| **Solution Optimality** | Guaranteed optimal (5 steps) | Non-optimal path | BFS guarantees the shallowest goal state[cite: 11] |
| **Completeness** | Complete | Complete (with $d=15$ cutoff) | DFS requires depth-bounding to avoid runaway branches[cite: 10, 11] |

---

## 📁 4. Repository Structure

```text
├── bfs_vs_dfs.py             # Primary search logic and quantitative timeit benchmarking
├── profile_flamegraph.py     # High-iteration workload harness for py-spy sampling
├── flamegraph_bfs.svg        # Interactive CPU call-stack flamegraph for BFS
├── flamegraph_dfs.svg        # Interactive CPU call-stack flamegraph for DFS
├── .gitignore                # Excludes local Word reports (.docx) and cache artifacts
└── README.md                 # System overview and C4 architecture documentation
```

---

## 🚀 5. Execution & Verification

### Running the Quantitative Benchmark
```powershell
python bfs_vs_dfs.py
```

### Capturing Call-Stack Profiling Flamegraphs (`py-spy`)
```powershell
# Profile BFS Execution Stack
& "py-spy" record -o flamegraph_bfs.svg -- python profile_flamegraph.py bfs

# Profile DFS Execution Stack
& "py-spy" record -o flamegraph_dfs.svg -- python profile_flamegraph.py dfs
```
