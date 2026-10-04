# C4 Level 4 – Code-Level Overview

The code-level architecture represents the core functions, data structures, and global configuration definitions used across `bfs_vs_dfs.py` and `profile_flamegraph.py`[cite: 8, 9]:

```python
# Global Constants & Board Representation
START_STATE = (1, 2, 3, 0, 4, 6, 7, 5, 8)
GOAL_STATE  = (1, 2, 3, 4, 5, 6, 7, 8, 0)
MOVES       = [(-1, 0), (1, 0), (0, -1), (0, 1)]  # Up, Down, Left, Right

def get_neighbors(state: tuple) -> list[tuple]:
    """Generates all valid successor board states by sliding the empty tile (0)."""
    ...

def bfs(start: tuple, goal: tuple) -> tuple[bool, int, list]:
    """Explores states level-by-level using a FIFO collections.deque queue."""
    ...

def dfs(start: tuple, goal: tuple, max_depth: int = 15) -> tuple[bool, int, list]:
    """Explores branch paths deeply using a LIFO list stack with a depth limit."""
    ...

def compare_search_methods(runs: int = 5) -> None:
    """Measures and benchmarks runtime latency using timeit and prints performance metrics."""
    ...

def run_workload() -> None:
    """Executes high-iteration search loops to provide sampling frames for py-spy flamegraphs."""
    ...
```

---

## Code Responsibilities
* `START_STATE`, `GOAL_STATE`, `MOVES`: Define immutable tuple representations of the initial board, target layout, and legal matrix translations for the empty space (`0`)[cite: 9].
* `get_neighbors(state)`: Uses `divmod` on the zero-index to compute grid rows/columns, checks boundaries, and yields valid adjacent board tuples[cite: 9].
* `bfs(start, goal)`: Executes Breadth-First Search using `collections.deque` and an explored `set` to guarantee shortest-path optimality[cite: 9].
* `dfs(start, goal, max_depth=15)`: Executes Depth-First Search with a bounded depth limit ($d=15$) to prevent infinite cycles along deep branches[cite: 9].
* `compare_search_methods()`: Gathers execution times and counts expanded nodes to generate comparative benchmark tables[cite: 9].
* `run_workload()`: Feeds high-volume repeated executions to `py-spy` for non-intrusive CPU sampling and interactive SVG flamegraph generation[cite: 7].
