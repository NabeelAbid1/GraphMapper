# PathFinder

A cybersecurity-focused DSA project that models a computer network as a **directed weighted graph** to simulate and analyze real-world attack paths between systems.

> Inspired by **BloodHound** — the industry-standard attack-path analysis tool used by penetration testers and security teams worldwide.

---

## Project Overview

PathFinder represents an entire computer network as a graph of relationships. **Hosts, users, services, and vulnerabilities become nodes.** Trust relationships, admin rights, credential reuse, and active sessions become weighted edges.

Given an attacker's entry point and a high-value target, PathFinder finds every possible attack path — and ranks them by how easy they are to exploit.

This is **not** an exploitation or hacking tool. It is a simulation and analysis engine built entirely on custom-implemented data structures and graph algorithms — no libraries, no shortcuts.

---

## Attack Path Example

```
Hacker Laptop ──(exploit, 2)──► Web Server ──(cred reuse, 3)──► Admin User
                                                                      │
                                                              (admin_to, 1)
                                                                      ▼
                  Laptop1 ◄──(has_session, 2)────────────── File Server

Total Cost: 8  |  Hops: 4  |  Found by: Dijkstra's Algorithm
```

---

## Core Data Structures (Built from Scratch)

| Structure | Role in PathFinder |
|-----------|-------------------|
| **Hash Table** | O(1) node lookup by ID — no linear searching |
| **Graph (Adjacency List)** | Core network map — each node stores its outgoing edges |
| **Queue** | Powers BFS — explores the network layer by layer |
| **Stack** | Powers DFS — dives deep into a path before backtracking |
| **Min-Heap** | Powers Dijkstra — always expands the cheapest path first |

---

## Algorithms

| Algorithm | Question it Answers | Why it Matters |
|-----------|-------------------|----------------|
| **BFS** | Is the target reachable? What's the minimum hops? | Catches any connection — even indirect ones humans miss |
| **DFS** | What are *all* possible paths to the target? | Reveals every route an attacker could take |
| **Dijkstra** | What is the *cheapest* (easiest) attack path? | Mimics real attacker logic — chain easy steps, avoid hard ones |

> **Key insight:** BFS and Dijkstra intentionally disagree. BFS finds the *shortest* path (fewest hops). Dijkstra finds the *cheapest* path (lowest total cost). In security, these are rarely the same — and that difference is the whole point.

---

## Graph Model

**Node Types**
- `HOST` — Physical or virtual machines (laptops, servers, domain controllers)
- `USER` — Account identities with permissions and group memberships
- `SERVICE` — Running services (RDP, SSH, HTTP, SMB)
- `VULNERABILITY` — Known weaknesses with associated exploit difficulty

**Edge Types (Relationships)**

| Edge | Meaning | Typical Cost |
|------|---------|-------------|
| `admin_to` | Account has admin rights on a host | Low (1–2) |
| `has_session` | Active login session exists | Low–Medium (2–3) |
| `credential_reuse` | Password found here works elsewhere | Medium (3–4) |
| `exploits_vuln` | Known vulnerability can be exploited | Medium–High (3–8) |
| `connects_to` | Network-level connectivity | Varies |

---

## DSA Concepts Applied

| Concept | Implementation |
|---------|---------------|
| **Directed Weighted Graph** | Network relationships have direction and exploit difficulty cost |
| **Hash Table (Chaining)** | Node registry — resolves collisions via linked lists |
| **Min-Heap (Priority Queue)** | Dijkstra's frontier — always processes lowest-cost node next |
| **Adjacency List** | Memory-efficient graph storage for sparse security graphs |
| **BFS (Level-order traversal)** | Reachability analysis and hop-count minimization |
| **DFS (Backtracking)** | Full path enumeration and cycle detection in trust chains |

---

## Input Format

Network data is loaded from a **JSON file — not hardcoded.** Swap the file, get a different network. No code changes required.

```json
{
  "nodes": [
    { "id": "HL",         "type": "HOST", "label": "Hacker Laptop",   "risk": 1 },
    { "id": "WebServer",  "type": "HOST", "label": "Web Server",       "risk": 3 },
    { "id": "AdminUser",  "type": "USER", "label": "Admin Account",    "risk": 4 },
    { "id": "L1",         "type": "HOST", "label": "Laptop1",          "risk": 5 }
  ],
  "edges": [
    { "from": "HL",        "to": "WebServer", "type": "exploits_vuln",     "cost": 2 },
    { "from": "WebServer", "to": "AdminUser", "type": "credential_reuse",  "cost": 3 },
    { "from": "AdminUser", "to": "L1",        "type": "admin_to",          "cost": 1 }
  ]
}
```

---

## Project Structure

```
PathFinder/
├── src/
│   ├── data_structures/   # HashTable, Graph, Queue, Stack, MinHeap
│   ├── algorithms/        # BFS, DFS, Dijkstra
│   ├── models/            # Node and Edge class definitions
│   └── utils/             # JSON loader, path printer, helpers
├── data/
│   ├── small_network.json     # 5-node basic scenario
│   ├── medium_network.json    # Multi-path scenario (BFS vs Dijkstra disagree)
│   └── tricky_network.json    # Dead ends, cycles, no-path cases
├── tests/                 # Unit tests per data structure and algorithm
├── docs/                  # Design diagrams and notes
└── README.md
```

---

## Demo Workflow

```
1. Load network from JSON
2. Display all nodes and edges
3. Select: Start Node → Target Node
4. Run BFS   → "Fewest hops path: HL → WebServer → AdminUser → L1  (3 hops)"
5. Run DFS   → "All paths found: 2 total paths to L1"
6. Run Dijkstra → "Cheapest path: HL → WebServer → AdminUser → L1  (cost: 6)"
7. Live edit: add new node to JSON → rerun → new paths appear instantly
```

---

## Key Design Decisions

✓ **No external graph libraries** — every structure coded from scratch  
✓ **Directed edges** — `A → B` does not imply `B → A`  
✓ **Weighted edges** — fewer hops ≠ easier path (Dijkstra vs BFS)  
✓ **JSON-driven input** — fully dynamic, not hardcoded  
✓ **Multiple test scenarios** — edge cases built into demo data  

---

## Status

- [x] Project structure set up
- [ ] Hash Table implemented
- [ ] Graph (Adjacency List) implemented
- [ ] Queue and Stack implemented
- [ ] Min-Heap implemented
- [ ] BFS implemented
- [ ] DFS implemented
- [ ] Dijkstra implemented
- [ ] JSON loader
- [ ] CLI interface
- [ ] Demo scenarios (small / medium / tricky)
- [ ] Path visualization (stretch goal)

---

## Future Enhancements

- Nmap XML import — parse real network scan output into the graph
- Visual graph rendering — highlight attack path in color
- "Most critical node" analysis — which node, if removed, breaks the most paths
- Risk scoring per path — composite score beyond just edge cost

---

## Technical Stack

- **Language:** C++
- **Paradigm:** Data Structures & Algorithms — all implemented manually
- **Input:** JSON
- **Interface:** Console CLI (menu-driven)
- **Visualization:** *(Stretch — matplotlib / networkx)*

---

## Author

**Nabeel Abid**  
GitHub:[@NabeelAbid1](https://github.com/NabeelAbid1)

**Sheraz Ali**  
GitHub: [@Sheraz-Ali403](https://github.com/Sheraz-Ali403)

---

## Not a Hacking Tool

PathFinder is an **analysis and simulation tool only.** It operates on static JSON data you define. It performs no live network scanning, no exploitation, and no unauthorized access of any kind.

---

*Built as a semester DSA project — a simplified implementation of concepts used by real tools like [BloodHound](https://github.com/BloodHoundAD/BloodHound) by SpecterOps.*
