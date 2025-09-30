# GISelle, Safety-Aware Navigation and Pathfinding Platform for Women (C++, EZGL, Multi-Threaded Algorithms)

GISelle (from elle, French for “her”) is an interactive, safety-focused navigation platform built as part of the University of Toronto's ECE297: Software Design & Communication course.
It integrates advanced Geographic Information System (GIS) algorithms with real-time performance optimizations and a user-focused design, aiming to make urban travel not just faster, but safer and more intuitive — especially for women and vulnerable communities. Awarded an **A** for exceptional algorithmic performance, innovation, and usability testing across all milestones.

## Final Demo + UI Showcase (wait for GIF to load...)

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/12b077a3-ea3a-4f56-9ac9-181446036022" alt="Watch the Full Demo" width="900"/>
  </a>
</p>


---

## Why GISelle?

Most mapping apps prioritize shortest distance or travel time, often ignoring critical safety constraints. GISelle redefines navigation by:
- Incorporating safety-aware heuristics such as lighting conditions and main-road bias.
- Supporting emergency rerouting (e.g., nearest police station) in real-time.
- Offering shift-based UI modes (day/night) tailored for high-risk urban scenarios.

---

## Development Overview
- GISelle was developed across four major milestones, each introducing new algorithms, optimization techniques, and usability improvements. Below is a breakdown of these phases, with live demos and visual snapshots.
---

## Milestone 1: Data Integration & Basic Rendering
Goal:
Establish a foundational GIS engine by processing .bin map data into a structured format for fast spatial lookups and rendering the map via a custom EZGL GUI framework.
Core Focus:
- Parsing map data for streets, intersections, and points of interest (POIs).
- Building efficient adjacency structures for graph-based navigation.
---
## Milestone 2: Intelligent Search & Icon Filtering

Goal:
- Deliver real-time street search and context-aware visual enhancements, improving both speed and readability.

---
### UI Before & After: Day, Night, and Shift Modes
** Before GISelle Enhancements (Basic UI):**  
Low contrast, minimal feedback, and no safety-specific filters.
| Old UI Preview |
|----------------|
| ![Old UI](https://github.com/user-attachments/assets/af1c83a5-c154-4dca-b105-04f8dac1bcf7) |
---
#### No Shift Applied, Normal Map After Rendering  
- The clean, default GISelle interface after core Milestone 2 features were implemented — including search, drawing functions, and initial icon placements.  
This version includes streets, intersections, and POIs rendered via EZGL, but without night or shift-specific styling.
![Raw Map](https://github.com/user-attachments/assets/b0281ca4-620a-4d75-a548-657350177283)
---
#### After (Night Shift Mode)  
- Highlighting essential services for late-night users like paramedics, students, or women traveling alone.  
- This mode emphasizes police stations, hospitals, and transit hubs with high-contrast icons and a darker color palette for visual comfort and rapid decision-making in low-light conditions.
![Screenshot (663)](https://github.com/user-attachments/assets/48414b11-9707-4e36-aaac-da56bbc60577)
---
#### After (Dark Mode)  
Designed for users navigating in the evening or indoors.  
Dark Mode reduces glare while preserving all default functionalities — including search, icons, and map interaction — in a visually accessible format.
![Dark Mode UI](https://github.com/user-attachments/assets/0f6687e5-fde6-469c-bf66-03c62842bade)

### Icon Filtering System
**Why it matters:**  
In safety-critical environments, clarity and speed matter. Our system allows users to locate police stations, hospitals, and transit hubs with easily scannable icons.
**Features:**
- Icons designed with **distinct night/day modes**.
- Shift-aware coloring for **night-shift workers**.
- Designed for visual accessibility with minimal cognitive load.

<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/657905b9-fd1b-4382-acfc-73becb07c3f2" alt="Watch the Full Demo" width="900"/>
  </a>
</p>

---

#### After (Night Mode + Icon Filters)  
- Night Mode introduces a dark-themed interface optimized for low-light environments, helping reduce eye strain while maintaining all map functionalities.  
- Tailored safety icons make it easier to identify critical locations — even at a glance.

**Police Stations, Hospitals, and Transit Points** are highlighted with high-contrast icons and hover tooltips for fast navigation.

**Full UI Preview:**  
![Night UI](https://github.com/user-attachments/assets/61dd6bf4-6028-4be0-8d2e-15b2680e4928)

**Icon Filter Preview (Active):**  
![Icons](https://github.com/user-attachments/assets/1fd3456f-6e00-4236-95f3-7d07f70f8177)


### Smart Autocomplete System

Core Highlights:
- Developed a custom **Trie** data structure to handle real-time prefix-based search.
- Supported both full and partial street name matches, improving UX for urgent or unclear queries.
- Connected search results to interactive map highlights, guiding users visually.

 Autocomplete in Action:
![AutoComplete Result](https://github.com/user-attachments/assets/06da75a2-27ab-4a32-83ea-b96a4fb211db)

---

##  Milestone 3: Smart Pathfinding with A* Algorithm

**Goal:**  
Design a responsive, real-world routing system that considers time, safety, and usability — built around the classic **A\*** search algorithm.

---
###  Real-World Routing with A*

To move beyond just "shortest distance," our implementation of A\* prioritized:
-  **Time-Based Cost Function**  
  Prioritized realistic arrival times over physical distance.
- **Turn Penalties**  
  Added time penalties at intersections to simulate real-world travel friction (e.g., waiting at red lights or complex junctions).
- **Main Road Bias**  
  Weighted larger, well-lit roads more heavily for safety and visibility — especially at night.
> **Technical Insight:**  
> We used a priority queue that balances `g(n)` (actual cost from start) and `h(n)` (heuristic estimate to goal).  
> This drastically reduced the number of nodes explored, maintaining both speed and optimality.

---

### Use Case: Finding the Shortest Safe Route

**Scenario 1:**  
User selects two intersections and requests the optimal route.  
The system computes and displays the most efficient, **safest** path — factoring in road types, turn penalties, and time.

**Shortest Path Between 2 Intersections:**  
![Shortest Path](https://github.com/user-attachments/assets/b1b35238-5ac3-4bed-8b18-c45324f1cd41)

---

### Use Case: Emergency Re-Routing to Police

**Scenario 2:**  
After selecting a route, the user clicks the **“Find Nearest Police Station”** button.  
GISelle then reroutes to the closest available police station, ideal for emergency situations.

**Shortest Path + Nearest Police Station Redirect:**  
![Police Station Reroute](https://github.com/user-attachments/assets/ca1bc35e-8639-424c-a373-64ed178a19c5)

---

## Milestone 4: Traveling Courier Problem

Goal: 
Solve a multi-pickup, multi-dropoff delivery route problem under depot and time constraints using **metaheuristics**.

Core Focus:
- Used **Multi-Destination Dijkstra** to precompute travel times.
- Applied **Multi-Start + 2-Opt Simulated Annealing (SA)** for optimization.
- Enabled **parallelism** using `std::thread` for concurrent heuristic execution under 50s time constraints.

---

## Final Features: Safety-Focused Mapping

**Goal:**  
Extend GISelle’s core functionality to directly support **real-time responsiveness** and **women’s safety** — especially in unfamiliar or high-risk areas.

---

### Key Features

- **Find-Nearest-Police Button**  
  Allows users to instantly reroute to the closest police station in an emergency.  
<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/f7d23e41-63be-420e-bd48-4e1a8c5584d0" alt="Watch the Full Demo" width="900"/>
  </a>
</p>
 

  - **Helpline Toggle by Region**  
  Dynamically displays regional helplines, offering support hotlines tailored to the user’s location.  
<p align="center">
  <a href="https://drive.google.com/file/d/1ZqqvSZlfG-oxGxM2vKJ8t9Unu_PgmEsV/view?usp=sharing">
    <img src="https://github.com/user-attachments/assets/2f5e0292-2f11-44ef-933b-97705226e1b6" alt="Watch the Full Demo" width="900"/>
  </a>
</p>


- **User Testing & Feedback**  
  Conducted **System Usability Scale (SUS)** surveys and timed user trials to validate speed, clarity, and reliability of safety features.

---

### Future Additions

- **Street-Light-Aware Routing**  
  Integrate street-light data for safer nighttime route planning.

- **Uber API Integration (Women Rider Preference)**  
  Enable ride-hailing directly from map UI, respecting safety preferences.

- **Crowdsourced Safety Flagging**  
  Let users tag unsafe areas or report incidents, backed by backend moderation.

---

> _Safety shouldn’t be an afterthought — GISelle puts it at the center of every map interaction._


## Testing + Results

- **Usability:**  
  System Usability Scale (SUS) scores from 10 participants showed high confidence and ease-of-use.

- **Responsiveness:**  
  Most users found safety buttons in under **2 seconds**.

- **Courier Optimization:**  
  Route time improved by **10–15 seconds** using multi-start and custom 2-opt.

---

## Meet the Team Behind GISelle

this was my amazing team :D
![Team Photo](https://github.com/user-attachments/assets/1a87ed0e-7e69-4092-959c-0e9b3d8d6b8d)
