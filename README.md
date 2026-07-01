<div align="center">

# 🌕 AstroNav × ISRO BAH 2026
### Problem Statement 08 — Subsurface Ice Detection, Lunar South Polar Region

[![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)](https://docker.com)
[![HuggingFace](https://img.shields.io/badge/🤗%20HuggingFace-Live-yellow)](https://Innoventors11-openenv-hackathon.hf.space)
[![BAH2026](https://img.shields.io/badge/ISRO-BAH%202026-orange)](https://hack2skill.com/event/bah2026/)
[![Status](https://img.shields.io/badge/Status-Idea%20Submitted-green)]()

<br/>

**Team Innoventors** · Bharatiya Antariksh Hackathon 2026 · PS 08

*Adapting our Mars drone navigation AI to plan safe rover traverses on the Moon's south pole — using real Chandrayaan-2 radar data.*

[Live Demo](https://Innoventors11-openenv-hackathon.hf.space) · [API Docs](https://Innoventors11-openenv-hackathon.hf.space/docs) · [Idea Submission PPT](./docs/resources/) · [Team Tasks](./docs/TEAM_TASKS.md)

</div>

---

## 📋 Table of Contents

- [What We're Building](#-what-were-building)
- [Problem Statement 08 — Plain Words](#-problem-statement-08--plain-words)
- [Our Approach](#-our-approach)
- [How AstroNav Connects to PS 08](#-how-astronav-connects-to-ps-08)
- [Architecture](#-architecture)
- [Team](#-team)
- [Quick Start](#-quick-start)
- [Repository Structure](#-repository-structure)
- [Documentation](#-documentation)
- [Timeline & Milestones](#-timeline--milestones)
- [Resources](#-resources)

---

## 🚀 What We're Building

We are building an AI-powered **Lunar Mission Planner** that:

1. Takes **Chandrayaan-2 DFSAR radar + OHRC imagery** from ISRO's PRADAN portal
2. Converts it into a navigable **terrain grid** with ice probability and hazard layers
3. Runs our **reinforcement-learning navigation agent (AstroNav)** on that grid
4. Outputs a **ranked list of landing sites** and a **recommended rover traverse path**

> **Why us?** Most teams start from scratch. We already have a working, benchmarked RL navigation engine — AstroNav — that passed all three difficulty levels (Easy: 0.55 ✅ Medium: 0.41 ✅ Hard: 0.35 ✅). We are adapting a proven system, not building blind.

---

## 🎯 Problem Statement 08 — Plain Words

> *Full title: "Detection and Characterization of Subsurface Ice in Lunar South Polar Regions Using Chandrayaan-2 Radar and Imagery Data for Landing Site and Rover Traverse Planning"*

| Term | What it actually means |
|------|----------------------|
| **Lunar South Polar Region** | Moon's south pole — permanently shadowed craters, freezing cold, where Chandrayaan-3 landed in 2023 |
| **Subsurface ice** | Water ice buried underground — invisible to cameras, detectable only via radar |
| **Chandrayaan-2 DFSAR** | Dual Frequency Synthetic Aperture Radar — India's own satellite instrument that fires radar waves at the Moon's surface and reads the bounce-back signal to detect what's underground |
| **OHRC imagery** | Optical high-res camera on Chandrayaan-2 — for visual terrain mapping (craters, slopes, shadows) |
| **Landing site planning** | Choosing the safest spot for a future mission to land |
| **Rover traverse planning** | Planning the path a rover should drive after landing to reach ice sites safely |

**One-sentence summary:** Use ISRO's own satellite data + AI to tell a future rover *where* ice is hidden, *where* to land, and *which path* to drive.

📖 **Read more:** [`docs/PS08_BRIEF.md`](./docs/PS08_BRIEF.md)

---

## 🧠 Our Approach

```
Chandrayaan-2 Data  →  Preprocessing  →  Ice + Hazard Map  →  RL Agent  →  Rover Path + Landing Sites
     (DFSAR + OHRC)      (CPR analysis)     (grid format)       (AstroNav)    (final output)
```

### Step-by-step

**Step 1 — Get the data**
Download Chandrayaan-2 DFSAR and OHRC data from [ISRO PRADAN portal](https://pradan.issdc.gov.in/).
The DFSAR data gives us radar backscatter values. High Circular Polarization Ratio (CPR) = likely ice.

**Step 2 — Convert to a grid**
Rasterize the radar image into a 2D grid (same structure as AstroNav's environment).
Each cell gets tagged: `ICE_PROBABLE` / `SAFE_TERRAIN` / `HAZARD` (crater/slope/shadow).

**Step 3 — Ice probability mapping**
Apply CPR threshold analysis on DFSAR data. Cross-reference with OHRC imagery to flag permanently shadowed regions and steep slopes as hazards.

**Step 4 — Run the RL agent**
Feed the grid into AstroNav. The agent already knows how to:
- Explore unknown terrain (+2 reward per new cell)
- Detect targets (+10 reward — now: ice-probable cells)
- Avoid obstacles (-15 reward — now: craters + hazards)
- Return home safely (+20 reward — now: reach landing site from ice zone)

**Step 5 — Generate output**
- Ranked list of **candidate landing sites** (ice score × safety score)
- Recommended **rover traverse path** from landing site to ice deposits
- Interactive dashboard visualizing the lunar grid

---

## 🔗 How AstroNav Connects to PS 08

This is the core of our idea — every component we already built has a direct real-world counterpart.

| AstroNav (what we built) | PS 08 (what ISRO needs) |
|--------------------------|--------------------------|
| 2D grid — the Mars map | Chandrayaan-2 radar image converted to a grid |
| `T` — geological target | Subsurface ice deposit location |
| `X` — obstacle | Crater / permanently shadowed region / steep slope |
| `B` — base station | Designated landing site |
| Battery management logic | Solar power constraints on a real rover |
| Reward: +2 new cell | Reward: +2 exploring new lunar terrain |
| Reward: +10 target found | Reward: +10 ice-probable cell reached |
| Reward: -15 obstacle crash | Reward: -15 entering hazard zone |
| Reward: +20 safe return | Reward: +20 rover safely back to landing site |
| `astronav_env.py` | Lunar environment wrapper (new file we build) |
| `inference.py` | Path recommendation engine (adaptation) |

> **Translation:** AstroNav is the brain. Chandrayaan-2 data is the new set of eyes.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     DATA LAYER                           │
│   Chandrayaan-2 DFSAR   │   OHRC Imagery   │  PRADAN   │
└──────────────────┬──────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────┐
│                  PROCESSING LAYER                        │
│   Image preprocessing   │   CPR analysis   │  Grid      │
│   (rasterio / OpenCV)   │   (ice mapping)  │  mapper    │
└──────────────────┬──────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────┐
│                  AI / RL LAYER                           │
│   AstroNav RL engine (policy + reward function)         │
│   Ice & hazard classifier                               │
└──────────────────┬──────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────┐
│                  OUTPUT LAYER                            │
│   Ranked landing sites  │  Rover path  │  Dashboard     │
└─────────────────────────────────────────────────────────┘
```

---

## 👥 Team

| Role | Name | Responsibility |
|------|------|----------------|
| 🏅 **Team Leader** | Yashika Soni | Project architecture, RL environment, coordination |
| 🤝 **Member 1** | `[TBD]` | `[TBD — see TEAM_TASKS.md]` |
| 🤝 **Member 2** | `[TBD]` | `[TBD — see TEAM_TASKS.md]` |
| 🤝 **Member 3** | `[TBD — optional]` | `[TBD]` |

📋 **Task breakdown with deadlines:** [`docs/TEAM_TASKS.md`](./docs/TEAM_TASKS.md)

---

## ⚡ Quick Start

### Prerequisites
- Python 3.11+
- Docker (optional but recommended)
- Git

### 1. Clone the repo
```bash
git clone https://github.com/yashi057/INNOVENTORS.git
cd INNOVENTORS
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the existing AstroNav baseline
```bash
python inference.py
```

### 4. Start the API server
```bash
uvicorn main:app --host 0.0.0.0 --port 7860
# Open: http://localhost:7860/docs
```

### 5. Run with Docker
```bash
docker build -t astronav .
docker run -p 7860:7860 astronav
```

📖 **Detailed setup:** [`docs/SETUP.md`](./docs/SETUP.md)

---

## 📁 Repository Structure

```
INNOVENTORS/
│
├── environment/
│   ├── astronav_env.py        ← Existing Mars RL environment
│   └── lunar_env.py           ← NEW: Lunar adaptation for PS 08
│
├── data/
│   ├── raw/                   ← Raw Chandrayaan-2 DFSAR + OHRC files
│   └── processed/             ← Processed grid files
│
├── pipeline/
│   ├── preprocess.py          ← NEW: Radar image → grid converter
│   ├── ice_mapper.py          ← NEW: CPR analysis + ice probability
│   └── hazard_mapper.py       ← NEW: Crater + shadow detection
│
├── models/
│   └── trained_agent/         ← Saved RL model checkpoints
│
├── main.py                    ← FastAPI server
├── inference.py               ← Baseline agent runner
├── grader.py                  ← Scoring logic
├── requirements.txt
├── Dockerfile
│
└── docs/
    ├── PS08_BRIEF.md          ← Full PS 08 explanation (read this first!)
    ├── SETUP.md               ← Detailed setup guide
    ├── TEAM_TASKS.md          ← Who does what + deadlines
    └── resources/             ← PPT, papers, data links
```

---

## 📚 Documentation

| File | What's in it | Read if you... |
|------|-------------|----------------|
| [`docs/PS08_BRIEF.md`](./docs/PS08_BRIEF.md) | Full PS 08 explanation from scratch — RL, grid, Chandrayaan-2, all of it | Just joined the team / need full context |
| [`docs/SETUP.md`](./docs/SETUP.md) | Environment setup, data download, running the project | Setting up locally for the first time |
| [`docs/TEAM_TASKS.md`](./docs/TEAM_TASKS.md) | Task tracker — who owns what, deadlines, current status | Want to know what to work on |

---

## 🗓️ Timeline & Milestones

| Phase | What | Deadline | Status |
|-------|------|----------|--------|
| **Phase 1** | Idea submission (PPT) | July 1, 2026 | ✅ Done |
| **Phase 2** | Chandrayaan-2 data download + preprocessing pipeline | TBD by ISRO | 🔄 In progress |
| **Phase 3** | Lunar RL environment adaptation | TBD | ⏳ Pending |
| **Phase 4** | Ice probability + hazard mapping | TBD | ⏳ Pending |
| **Phase 5** | Integration + dashboard | TBD | ⏳ Pending |
| **Phase 6** | Final submission + demo | TBD | ⏳ Pending |

---

## 🔗 Resources

### Official
- 🏛️ [BAH 2026 Event Page](https://hack2skill.com/event/bah2026/)
- 🛰️ [ISRO PRADAN Data Portal](https://pradan.issdc.gov.in/) — Chandrayaan-2 DFSAR + OHRC data download
- 📄 [Chandrayaan-2 DFSAR Paper (ISRO, 2023)](https://www.isro.gov.in/) — ice detection using CPR

### Our Project
- 🤗 [Live AstroNav Demo](https://Innoventors11-openenv-hackathon.hf.space)
- 📚 [AstroNav API Docs](https://Innoventors11-openenv-hackathon.hf.space/docs)
- 💻 [GitHub Repo](https://github.com/yashi057/INNOVENTORS)

### Learning (if you're new to RL or radar data)
- 🎓 Start with `docs/PS08_BRIEF.md` — explains everything from zero
- 📖 [Stable Baselines3 Docs](https://stable-baselines3.readthedocs.io/) — the RL library we'll use for training
- 🗺️ [rasterio Quickstart](https://rasterio.readthedocs.io/) — for reading radar image files

---

<div align="center">

**Team Innoventors** · BAH 2026 · PS 08

*Built on AstroNav — from Mars to the Moon 🚀🌕*

</div>
