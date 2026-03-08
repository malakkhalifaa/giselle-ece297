# <u>GISelle</u> — Safety-Aware Navigation & Pathfinding Platform

**C++ · EZGL · A* · Multi-Destination Dijkstra · Simulated Annealing · Multi-threaded**

GISelle (*elle*, French for "her") is a safety-first GIS navigation platform developed for the University of Toronto **ECE297: Software Design & Communication** course. It extends traditional shortest-path routing with **safety-aware heuristics**, **emergency rerouting**, and **context-aware UI modes** for women and vulnerable users. The project received an **A** for algorithmic performance, implementation quality, and usability validation across all milestones.

---

## <u>Demo</u>

**[▶ Watch full demo (Google Drive)](https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing)**

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/12b077a3-ea3a-4f56-9ac9-181446036022" alt="Full demo" width="900"/>
  </a>
</p>

---

## <u>Motivation</u>

Standard routing engines optimize for **distance** or **travel time** and do not model safety. GISelle adds:

- **Safety-aware cost functions** — main-road bias, lighting, and turn penalties.
- **Emergency rerouting** — one-click redirect to the nearest police station.
- **Shift-aware UI** — day, night, and night-shift modes with high-contrast safety POIs (police, hospitals, transit).

---

## <u>Architecture & Milestones</u>

The system was built in four milestones: data pipeline and rendering, search and filtering, pathfinding, and delivery optimization.

---

### <u>Milestone 1: Data Integration & Rendering</u>

**Objective:** Build a GIS backend and renderer for interactive map display.

- **Input:** Binary (`.bin`) map dumps (streets, intersections, POIs, metadata); single-pass parse with fixed/variable-length fields.
- **Output:** In-memory directed graph — O(1) lookup by ID; adjacency list (intersection → outgoing segments; segment → from/to, length, one-way, name ID); spatial index for viewport culling.
- **Implementation:** Custom parser → adjacency structures → EZGL 2D rendering (draw order: streets, highlights, POI icons); pan/zoom via canvas transforms.

---

### <u>Milestone 2: Search & Context-Aware UI</u>

**Objective:** Real-time street search and safety-oriented visual modes.

#### Data structures & algorithms

- **Trie:** Prefix tree over street names; O(k) per keystroke; nodes store segment/intersection IDs for map highlight; full and partial match.
- **Icon filtering:** Per-type layers (police, hospitals, transit); day/night glyphs; visibility and z-order by UI mode; hover hit-test via POI bounds.

#### UI modes

| Mode        | Purpose                                                                 |
|-------------|-------------------------------------------------------------------------|
| **Default** | Full map; all POIs; baseline contrast.                                 |
| **Dark**    | Reduced glare; same features.                                          |
| **Night shift** | Police, hospitals, transit emphasized; darker palette; high-contrast POIs. |

**Before (baseline UI):** Low contrast, no safety-specific filters.

| Old UI |
|--------|
| ![Old UI](https://github.com/user-attachments/assets/af1c83a5-c154-4dca-b105-04f8dac1bcf7) |

**Default map (post–Milestone 2):** Streets, intersections, POIs via EZGL; search and drawing enabled.

![Default map](https://github.com/user-attachments/assets/b0281ca4-620a-4d75-a548-657350177283)

**Night shift mode:** Police, hospitals, transit highlighted; darker palette.

![Night shift](https://github.com/user-attachments/assets/48414b11-9707-4e36-aaac-da56bbc60577)

**Dark mode:** Same features; reduced glare.

![Dark mode](https://github.com/user-attachments/assets/0f6687e5-fde6-469c-bf66-03c62842bade)

**Icon filtering (demo):** [Full demo link](https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing)

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/657905b9-fd1b-4382-acfc-73becb07c3f2" alt="Icon filter demo" width="900"/>
  </a>
</p>

**Night mode + icon filters:**

![Night UI](https://github.com/user-attachments/assets/61dd6bf4-6028-4be0-8d2e-15b2680e4928)

![Icon filter active](https://github.com/user-attachments/assets/1fd3456f-6e00-4236-95f3-7d07f70f8177)

**Autocomplete (Trie-backed search):** Prefix search drives map highlighting.

![Autocomplete](https://github.com/user-attachments/assets/06da75a2-27ab-4a32-83ea-b96a4fb211db)

---

### <u>Milestone 3: A* Pathfinding</u>

**Objective:** Time- and safety-aware routing between two intersections.

**Cost model:** Edge cost = travel time (length/speed); main-road weighting; turn penalties at intersections (per in/out segment pair). Heuristic **h(n)** admissible (e.g. Euclidean or L1 to goal / max speed) for optimality.

**Implementation:** Min-heap open list keyed by **f(n)=g(n)+h(n)**; closed set (node IDs); path reconstruction via parent pointers; same adjacency list as M1.

**Shortest safe path (A→B):**

![Shortest path](https://github.com/user-attachments/assets/b1b35238-5ac3-4bed-8b18-c45324f1cd41)

**Emergency reroute to nearest police station:**

![Police reroute](https://github.com/user-attachments/assets/ca1bc35e-8639-424c-a373-64ed178a19c5)

---

### <u>Milestone 4: Traveling Courier Problem</u>

**Objective:** Multi-pickup, multi-dropoff routes with depot and time limits via metaheuristics.

- **Precomputation:** Multi-destination Dijkstra (or repeated from depot) → travel-time matrix **D[i][j]**; O(n²) storage, O(1) lookup in SA.
- **Optimization:** Multi-start + 2-opt Simulated Annealing; Metropolis acceptance; cooling schedule and iteration count tuned for ~50 s total.
- **Parallelism:** **std::thread** — N independent SA runs; join and take best solution; reduces variance within time cap.

---

## <u>Safety Features (Post–Milestone)</u>

- **Find nearest police:** A* or Dijkstra to all police nodes; pick min-cost; reroute and redraw.
- **Regional helplines:** Toggle shows crisis/support numbers for current region (config or embedded data).
- **Usability:** SUS (10 participants); timed tasks (e.g. find safety button &lt; 2 s).

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

## <u>Evaluation</u>

| Metric | Result |
|--------|--------|
| **Usability (SUS)** | 10 participants; high scores for confidence and ease of use. |
| **Responsiveness** | Majority located safety actions in &lt; 2 s. |
| **Courier (2-opt SA)** | ~10–15 s improvement vs baseline. |

---

## <u>Future Work</u>

- **Street-light–aware costs** — lighting data in edge weights for night routes.
- **Ride-hail API** — women/preferred-rider flow from map UI.
- **Crowdsourced safety** — user-reported incidents with moderation.

---

> *Safety shouldn't be an afterthought — GISelle centers it in every route and UI decision.*

---

