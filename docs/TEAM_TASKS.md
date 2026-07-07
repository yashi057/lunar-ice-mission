# 👥 Team Tasks & Role Assignments

> This is our living task tracker. Update your task status here as you make progress.
> **Rule:** Don't start something that's already in-progress by someone else. Check this file first.

---

## Team Structure

| # | Name | GitHub | Role | Status |
|---|------|--------|------|--------|
| 1 | **Yashika Soni** (Leader) | [@yashi057](https://github.com/yashi057) | RL environment, architecture, coordination | 🟢 Active |
| 2 | **Manisha Yadav** | [@manya011103](https://github.com/manya011103) | `[See tasks below]` | ⏳ TBD |
| 3 | **Kumkum Soni** | [@Kumkum-Mohit-07-06](https://github.com/Kumkum-Mohit-07-06) | `[See tasks below]` | ⏳ TBD |

---

## Phase 1 — Idea Submission ✅ DONE

| Task | Owner | Status |
|------|-------|--------|
| Write idea submission PPT | Yashika | ✅ Done |
| Submit on Hack2Skill portal | Yashika | ✅ Done |

---

## Phase 2 — Data & Preprocessing

> **Goal:** Download Chandrayaan-2 data and convert it into a grid our RL agent can read.

| # | Task | Description | Owner | Status | Notes |
|---|------|-------------|-------|--------|-------|
| 2.1 | Register on PRADAN | Create account on [pradan.issdc.gov.in](https://pradan.issdc.gov.in/) | Kumkum | ⏳ | Free account |
| 2.2 | Download DFSAR data | CH2-Dual Frequency SAR, south polar region, lat -89° to -85° | Kumkum | ⏳ | GeoTIFF or HDF5 format |
| 2.3 | Download OHRC imagery | CH2-OHRC optical images of same region | Kumkum | ⏳ | For crater/shadow detection |
| 2.4 | Write `preprocess.py` | Load + normalize radar data using `rasterio` | `[TBD]` | ⏳ | See `/pipeline/` folder |
| 2.5 | Write `ice_mapper.py` | CPR threshold analysis → flag ice-probable cells | `[TBD]` | ⏳ | CPR > 1.0 → probable ice |
| 2.6 | Write `hazard_mapper.py` | Crater + shadow detection from OHRC → flag hazard cells | `[TBD]` | ⏳ | |
| 2.7 | Write `grid_converter.py` | Merge ice + hazard maps into AstroNav-compatible grid | `[TBD]` | ⏳ | Output: 2D array same format as astronav_env.py |

---

## Phase 3 — Lunar RL Environment

> **Goal:** Wrap our Chandrayaan-2 grid inside an AstroNav-style RL environment.

| # | Task | Description | Owner | Status | Notes |
|---|------|-------------|-------|--------|-------|
| 3.1 | Create `lunar_env.py` | Adapt `astronav_env.py` for lunar terrain | Yashika | ⏳ | Swap random grid with real Chandrayaan-2 grid |
| 3.2 | Update reward function | Ice cells = +10, Hazards = -15, same battery logic | Yashika | ⏳ | |
| 3.3 | Run existing baseline on lunar grid | Test that `inference.py` works with new environment | Yashika | ⏳ | Should work with minimal changes |
| 3.4 | Train PPO/DQN agent | Use Stable-Baselines3 for proper training (not just greedy) | Yashika | ⏳ | `pip install stable-baselines3` |

---

## Phase 4 — Output & Dashboard

> **Goal:** Build the user-facing output: ranked landing sites + traverse path + visualization.

| # | Task | Description | Owner | Status | Notes |
|---|------|-------------|-------|--------|-------|
| 4.1 | Landing site ranker | Score each candidate site by ice_score × safety_score | `[TBD]` | ⏳ | Output: sorted list |
| 4.2 | Path extractor | Extract recommended traverse path from trained agent | `[TBD]` | ⏳ | |
| 4.3 | Visualization dashboard | Interactive map — grid view, ice zones, rover path | `[TBD]` | ⏳ | FastAPI + simple HTML/JS or Streamlit |
| 4.4 | API endpoints | `/recommend`, `/landing_sites`, `/traverse_path` | `[TBD]` | ⏳ | Add to existing `main.py` |

---

## Phase 5 — Integration & Final Submission

| # | Task | Description | Owner | Status |
|---|------|-------------|-------|--------|
| 5.1 | End-to-end pipeline test | Run full pipeline: data → grid → RL → output | Yashika | ⏳ |
| 5.2 | Docker update | Update Dockerfile for new dependencies | `[TBD]` | ⏳ |
| 5.3 | Deploy to HuggingFace Spaces | Update the existing HF Space with new lunar version | `[TBD]` | ⏳ |
| 5.4 | Final submission PPT | Update idea PPT with results/screenshots | Yashika | ⏳ |
| 5.5 | Video demo | Short demo video of the running system | `[TBD]` | ⏳ |

---

## How to Update Your Status

Edit this file directly on GitHub or via your local clone:

```
⏳ Pending   →  not started
🔄 In progress  →  actively working on it
✅ Done      →  complete and tested
❌ Blocked   →  stuck, needs help — add a comment below
```

If you're blocked, add a comment below the task row like:
```
> ❌ Blocked: can't download DFSAR data, getting auth error. @yashi057 help?
```

---

## Contribution Guide

```bash
# 1. Pull latest before starting any task
git pull origin main

# 2. Create a branch for your task
git checkout -b feature/ice-mapper

# 3. Do your work

# 4. Push and open a Pull Request
git push origin feature/ice-mapper
```

Commit message format:
```
feat: add CPR threshold analysis in ice_mapper.py
fix: correct grid dimensions in preprocess.py
docs: update TEAM_TASKS.md status
```

---

## Quick Links

- 📖 [PS08 Brief — read if you're new](./PS08_BRIEF.md)
- ⚙️ [Setup Guide](./SETUP.md)
- 🤗 [Live Demo](https://Innoventors11-openenv-hackathon.hf.space)
- 🛰️ [ISRO PRADAN Data Portal](https://pradan.issdc.gov.in/)
- 🏆 [BAH 2026 Event Page](https://hack2skill.com/event/bah2026/)
