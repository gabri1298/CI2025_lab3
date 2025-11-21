# 🕸️ Laboratory n° 3 — Computational Intelligence

## Overview

This laboratory focuses on solving the **All-Pairs Shortest Paths (APSP)** problem on weighted directed graphs.
The goal is to find the shortest path between every pair of nodes in a graph, handling various edge configurations including negative weights and detecting negative cycles.

The [problem-creator.ipynb](problem-creator.ipynb) notebook implements a comprehensive experimental study using **NetworkX** library to solve APSP across 224 different problem configurations.


---

## Project Structure

The notebook is organized into several sections, each responsible for a key component of the experimental approach:

- **Problem generator**
  Implements a configurable function to create weighted directed graphs with controllable properties: size, density, noise level, and support for negative edge weights.

- **Graph construction**
  Converts the generated problem matrix into a NetworkX directed graph, properly handling infinite values (representing disconnected edges).

- **All-pairs shortest path computation**
  Automatically selects the appropriate algorithm (Dijkstra for non-negative weights, Bellman-Ford for negative weights) and computes shortest paths between all node pairs.

- **Statistical analysis**
  Aggregates global metrics across all pairs: reachability ratios, path cost distributions (mean, median, min, max), and detection of negative cycles.

- **Experimental sweep**
  Systematically tests 224 combinations of parameters (size, density, noise, negative weights) and exports comprehensive results to CSV.


---

## Results

The results obtained in the notebook demonstrate the effectiveness and efficiency of using NetworkX for large-scale shortest path problems:

- The algorithms achieved **consistent performance** across all problem instances, even for graphs with 1000 nodes.
- **Algorithm selection** proved crucial for correctness:
  - **Dijkstra** was used for non-negative weights, providing fast execution times (< 10 seconds even for 1000-node graphs).
  - **Bellman-Ford** was necessary for negative weights, correctly detecting negative cycles when present.
- **Graph properties** significantly affected results:
  - **Higher density** improved reachability but increased computation time.
  - **Negative weights with high noise** frequently created negative cycles, making paths unbounded.
  - **Sparse graphs** had many unreachable pairs, especially at larger sizes.

### Why NetworkX Instead of A* or Custom Implementations?

The choice to use NetworkX was deliberate and based on fundamental algorithmic and practical considerations:

**1. The All-Pairs Problem Structure**
- The task requires computing shortest paths for **all n × (n-1) pairs** of nodes
- A 1000-node graph needs **999,000 individual path computations**
- A* is a **single-source, single-destination** algorithm—running it for all pairs would require 999,000 separate executions

**2. Specialized All-Pairs Algorithms Are Orders of Magnitude Faster**
- NetworkX uses algorithms specifically designed for all-pairs scenarios:
  - **Dijkstra** (non-negative weights): O(V·(V+E)·log(V)) for all pairs
  - **Bellman-Ford** (negative weights): O(V²·E) for all pairs
- These algorithms **share computation** across queries, avoiding redundant work
- Running A* n² times would be **computationally prohibitive** even with optimal implementations

**3. A* Requires Good Heuristics**
- A* needs **admissible heuristics** that never overestimate true cost
- For **general weighted graphs**, no universal heuristic exists that improves performance
- Without good heuristics, A* essentially degrades to Dijkstra anyway
- The generated graphs don't provide meaningful heuristics for arbitrary node pairs

**4. Scalability in Practice**
The experimental results validate this choice:
- **10 nodes**: < 0.001 seconds
- **100 nodes**: 0.01-0.20 seconds
- **1000 nodes**: 45-200 seconds (depending on density)
- A custom A*-based approach would take **hours** for the full 224-configuration sweep




