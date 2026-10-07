# Maze Route-Finding System: C4 Architectural Design (SLE-3)

> **Course:** 02AML204 - Introduction to Artificial Intelligence
> **Name:** Kushal Kumar Bhavi | **PRN:** 25UAM078
> **Date:** 01 October 2026

## Overview

A Maze Route-Finding System that takes a 2D grid maze with a start cell and a goal cell, and finds a route between them using **BFS** and **DFS**. It also profiles itself: it times each run, counts nodes expanded, and samples the call stack at 100 Hz to build a **flame graph**. It then reports the route, metrics and comparison charts.

This repository documents the system's architecture using the **C4 model** (Context, Container, Component, Code). It builds on the code from SLE-2.

## Architecture (C4 Model)

### Level 1: Context
The **User / Operator** gives a maze grid plus start and goal cells to the system. The system returns the route together with timing, node-count and flame-graph metrics as a path/report output. No external services are needed; everything runs locally.

### Level 2: Containers

| Container | Responsibility |
|---|---|
| **Input Module** | Loads the maze grid and start/goal cells, then passes them to the Search Engine |
| **Search Engine** | Runs BFS and DFS over the maze and produces a route |
| **Memory / Visited Set** | Stores the frontier and visited cells to avoid revisits and infinite loops |
| **Profiling & Metrics Module** | Times runs with `perf_counter()`, counts nodes expanded, samples the call stack at 100 Hz for the flame graph |
| **Output Module** | Combines route, timing table, node counts and flame graph into charts and a report |

### Level 3: Components (inside Search Engine)

```
Frontier (Open List)  ->  Neighbor Generator  ->  Goal Test  ->  Path Reconstructor
 queue (BFS)/stack (DFS)   get_neighbors()                         _reconstruct()
```

### Level 4: Code

| Function | File | Purpose |
|---|---|---|
| `get_neighbors(node, grid)` | route_finding.py | Returns valid, in-bounds, non-wall moves from a cell |
| `bfs(grid, start, goal)` | route_finding.py | Breadth-first search; returns `(path, nodes_expanded)` |
| `dfs(grid, start, goal, depth_limit)` | route_finding.py | Depth-limited DFS; returns `(path, nodes_expanded)` |
| `_reconstruct(came_from, start, goal)` | route_finding.py | Rebuilds the route from the `came_from` chain |
| `time_algorithm(fn, grid, start, goal)` | driver.py | Times a search function over several runs |
| `sampler_loop()` / `workload()` | flame_sampler.py | Background stack sampler and the BFS/DFS workload it samples |

## Design Decisions

- **Memory / Visited Set is its own container**, so it can be reused by another algorithm (e.g. A*) without changing the engine.
- **Profiling is separate from the Search Engine**, since it is an observability concern, not search logic. It can be turned off without touching the engine.
- **Goal Test and Neighbor Generator are separate components** because BFS and DFS share both unchanged. Only the Frontier's ordering (queue vs. stack) differs.

## Suggested Repository Structure

```
.
├── README.md
├── CONTRIBUTION_LOG.md
├── docs/
│   ├── SLE3_25UAM078_KushalKumarBhavi.docx
│   └── diagrams/
│       ├── context.png
│       ├── container.png
│       └── component.png
└── src/
    ├── route_finding.py
    ├── driver.py
    └── flame_sampler.py
```

## Conclusion

Adding C4 views on top of my SLE-2 code showed that BFS and DFS are really the same engine with a different frontier ordering, and that profiling and search are two separate concerns. It made the design decisions in my code explicit instead of implicit.

## AI Usage

Claude (Anthropic) was used as a design aid. See [CONTRIBUTION_LOG.md](CONTRIBUTION_LOG.md) for details.
