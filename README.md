# GISelle — Safety-Aware Navigation & Pathfinding Platform

**C++ · EZGL · A* · Multi-Destination Dijkstra · Simulated Annealing · Multi-threaded**

GISelle (*elle*, French for “her”) is a safety-first GIS navigation platform developed for the University of Toronto **ECE297: Software Design & Communication** course. It extends traditional shortest-path routing with **safety-aware heuristics**, **emergency rerouting**, and **context-aware UI modes** for women and vulnerable users. The project received an **A** for algorithmic performance, implementation quality, and usability validation across all milestones.

---

## Demo

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/12b077a3-ea3a-4f56-9ac9-181446036022" alt="Full demo" width="900"/>
  </a>
</p>

---

## Motivation

Standard routing engines optimize for **distance** or **travel time** and do not model safety. GISelle adds:

- **Safety-aware cost functions** — main-road bias, lighting, and turn penalties.
- **Emergency rerouting** — one-click redirect to the nearest police station.
- **Shift-aware UI** — day, night, and night-shift modes with high-contrast safety POIs (police, hospitals, transit).

---

## Architecture & Milestones

The system was built in four milestones: data pipeline and rendering, search and filtering, pathfinding, and delivery optimization.

---

### Milestone 1: Data Integration & Rendering

**Objective:** Build a GIS backend and renderer for interactive map display.

- **Input:** Binary (`.bin`) map dumps.
- **Output:** Structured graph (streets, intersections, POIs) with O(1) lookup for rendering and pathfinding.
- **Implementation:** Custom parser → adjacency structures (intersection ↔ street segments) → EZGL-based 2D rendering with pan/zoom.

---

### Milestone 2: Search & Context-Aware UI

**Objective:** Real-time street search and safety-oriented visual modes.

#### Data structures & algorithms

- **Trie** — prefix-based autocomplete for street names; supports partial and full matches and drives interactive map highlighting of search results.
- **Icon filtering** — toggleable layers for police stations, hospitals, and transit hubs with distinct day/night glyphs and tooltips.

#### UI modes

| Mode | Purpose |
|------|--------|
| **Default** | Full map; all POIs; baseline contrast. |
| **Dark** | Reduced glare; same features. |
| **Night shift** | Emphasizes police, hospitals, transit; darker palette for low-light use. |

**Before (baseline UI):**

| Old UI |
|--------|
| ![Old UI](https://github.com/user-attachments/assets/af1c83a5-c154-4dca-b105-04f8dac1bcf7) |

**Default map (post–Milestone 2):**

![Default map](https://github.com/user-attachments/assets/b0281ca4-620a-4d75-a548-657350177283)

**Night shift mode:**

![Night shift](https://github.com/user-attachments/assets/48414b11-9707-4e36-aaac-da56bbc60577)

**Dark mode:**

![Dark mode](https://github.com/user-attachments/assets/0f6687e5-fde6-469c-bf66-03c62842bade)

**Icon filtering (demo):**

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/657905b9-fd1b-4382-acfc-73becb07c3f2" alt="Icon filter demo" width="900"/>
  </a>
</p>

**Night mode + icon filters:**

![Night UI](https://github.com/user-attachments/assets/61dd6bf4-6028-4be0-8d2e-15b2680e4928)

![Icon filter active](https://github.com/user-attachments/assets/1fd3456f-6e00-4236-95f3-7d07f70f8177)

**Autocomplete (Trie-backed search):**

![Autocomplete](https://github.com/user-attachments/assets/06da75a2-27ab-4a32-83ea-b96a4fb211db)

---

### Milestone 3: A* Pathfinding

**Objective:** Time- and safety-aware routing between two intersections.

**Cost model:**

- **Edge cost:** Travel time (not just length); main roads favored for safety/visibility.
- **Turn penalties:** Extra cost at intersections (red lights, complex junctions).
- **Heuristic:** Admissible estimate to goal for A* optimality.

**Implementation:** Priority queue over `f(n) = g(n) + h(n)`; minimal node expansion while preserving optimality.

**Shortest safe path (A→B):**

![Shortest path](https://github.com/user-attachments/assets/b1b35238-5ac3-4bed-8b18-c45324f1cd41)

**Emergency reroute to nearest police station:**

![Police reroute](https://github.com/user-attachments/assets/ca1bc35e-8639-424c-a373-64ed178a19c5)

---

### Milestone 4: Traveling Courier Problem

**Objective:** Multi-pickup, multi-dropoff routes with depot and time limits, using metaheuristics.

- **Precomputation:** **Multi-destination Dijkstra** to build a travel-time matrix between all relevant nodes.
- **Optimization:** **Multi-start + 2-opt Simulated Annealing** to improve route quality under a 50 s runtime cap.
- **Parallelism:** **`std::thread`** to run multiple SA runs concurrently and keep the best solution.

---

## Safety Features (Post–Milestone)

- **Find nearest police** — one action reroutes the current path to the closest police station (with demo link below).
- **Regional helplines** — toggle shows crisis/support numbers for the current region (with demo link below).
- **Usability:** SUS surveys and timed tasks (e.g. “find safety button &lt; 2 s”) to validate clarity and responsiveness.

**Find nearest police (demo):**

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/f7d23e41-63be-420e-bd48-4e1a8c5584d0" alt="Find nearest police" width="900"/>
  </a>
</p>

**Helpline by region (demo):**

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/2f5e0292-2f11-44ef-933b-97705226e1b6" alt="Helpline toggle" width="900"/>
  </a>
</p>

---

## Evaluation

| Metric | Result |
|--------|--------|
| **Usability (SUS)** | 10 participants; high scores for confidence and ease of use. |
| **Responsiveness** | Majority located safety actions in &lt; 2 s. |
| **Courier (2-opt SA)** | ~10–15 s improvement in route time vs baseline. |

---

## Future Work

- **Street-light–aware costs** — integrate lighting data into edge weights for night routes.
- **Ride-hail API (e.g. Uber)** — optional women/preferred-rider flow from within the map UI.
- **Crowdsourced safety** — user-reported incidents or unsafe zones with moderation.

---

> *Safety shouldn’t be an afterthought — GISelle centers it in every route and UI decision.*

---

## Team

![Team](https://github.com/user-attachments/assets/1a87ed0e-7e69-4092-959c-0e9b3d8d6b8d)
