# GraphMapper

A cybersecurity-focused DSA project that models a computer network as a
directed weighted graph to analyze potential attack paths between systems.

> Built as a semester DSA project. Inspired by real tools like BloodHound.

---

## What it does

GraphMapper represents a network as a graph:
- **Nodes** — hosts, users, services, vulnerabilities
- **Edges** — relationships like `admin_to`, `has_session`, `credential_reuse`, `connects_to` (each with a cost/weight)

Given a start point (e.g. attacker's machine) and a target (e.g. domain controller), it finds and ranks possible attack paths using graph algorithms.

---

## Data Structures (implemented from scratch)

- Hash Table — fast node lookup by ID
- Graph (Adjacency List) — core network representation
- Queue — used by BFS
- Stack — used by DFS
- Min-Heap — used by Dijkstra as a priority queue

---

## Algorithms

| Algorithm | Question it answers |
|-----------|-------------------|
| BFS | Is the target reachable? How many hops minimum? |
| DFS | What are all possible paths to the target? |
| Dijkstra | What is the lowest-cost (easiest) attack path? |

---

## Project Structure

```
GraphMapper/
├── src/
│   ├── data_structures/   # Hash Table, Graph, Queue, Stack, Min-Heap
│   ├── algorithms/        # BFS, DFS, Dijkstra
│   ├── models/            # Node and Edge definitions
│   └── utils/             # JSON loader, helpers
├── data/                  # JSON network scenario files
├── tests/                 # Unit tests
├── docs/                  # Diagrams and design notes
└── README.md
```

---

## Input Format

Network data is loaded from a JSON file — not hardcoded.
You can edit or swap the JSON file without touching the code.
*(JSON schema will be documented here once finalized.)*

---

## Status

- [x] Project structure set up
- [ ] Data structures implemented
- [ ] Algorithms implemented
- [ ] JSON loader
- [ ] CLI interface
- [ ] Demo scenarios
- [ ] Visualization (stretch goal)

---

## Not a hacking tool

This is an **analysis and simulation** tool only. No live network scanning,
no exploitation. Input is static JSON data you define yourself.
