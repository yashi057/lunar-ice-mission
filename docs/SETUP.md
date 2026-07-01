# ⚙️ Setup Guide

> Follow this step-by-step to get the project running locally.
> Estimated time: 10–15 minutes.

---

## Prerequisites

Make sure you have these installed:

| Tool | Version | Check |
|------|---------|-------|
| Python | 3.11+ | `python3 --version` |
| Git | any | `git --version` |
| Docker | any (optional) | `docker --version` |
| pip | latest | `pip --version` |

---

## Step 1 — Clone the Repo

```bash
git clone https://github.com/yashi057/INNOVENTORS.git
cd INNOVENTORS
```

---

## Step 2 — Install Dependencies

```bash
pip install -r requirements.txt
```

**New dependencies for the PS 08 adaptation** (add these to requirements.txt as we build):

```bash
pip install rasterio          # for reading GeoTIFF radar files
pip install numpy opencv-python  # image processing
pip install stable-baselines3    # RL training (PPO/DQN)
pip install h5py              # for HDF5 format Chandrayaan-2 data
pip install matplotlib         # for visualization
```

---

## Step 3 — Run the Existing AstroNav Baseline

Before touching any new code, verify the existing project works:

```bash
python inference.py
```

You should see scores printed for Easy, Medium, and Hard difficulty levels.

Expected output:
```
Easy score:   0.55 ✅
Medium score: 0.41 ✅
Hard score:   0.35 ✅
Average:      0.44
```

If this works, you're good to go.

---

## Step 4 — Start the API Server

```bash
uvicorn main:app --host 0.0.0.0 --port 7860 --reload
```

Open your browser: `http://localhost:7860/docs`

You should see the Swagger UI with `/reset`, `/step`, and `/grade` endpoints.

---

## Step 5 — Docker (Optional but Recommended)

```bash
# Build
docker build -t astronav .

# Run
docker run -p 7860:7860 astronav

# API will be at http://localhost:7860/docs
```

---

## Step 6 — Download Chandrayaan-2 Data (Phase 2 onwards)

1. Go to [https://pradan.issdc.gov.in/](https://pradan.issdc.gov.in/)
2. Register for a free account
3. Search for: `Chandrayaan-2` → `DFSAR` instrument
4. Filter by region: Lunar South Polar (lat: -89° to -85°)
5. Download: GeoTIFF or HDF5 format
6. Place files in `data/raw/`

---

## Project Structure Reference

```
INNOVENTORS/
├── environment/
│   ├── astronav_env.py     ← DO NOT MODIFY (existing, working)
│   └── lunar_env.py        ← NEW: copy + adapt astronav_env.py for lunar grid
│
├── data/
│   ├── raw/                ← Put downloaded Chandrayaan-2 files here
│   └── processed/          ← Grid files go here after preprocessing
│
├── pipeline/               ← NEW folder: create these files
│   ├── preprocess.py
│   ├── ice_mapper.py
│   └── hazard_mapper.py
│
├── main.py                 ← FastAPI server (modify to add new endpoints)
├── inference.py            ← Baseline agent runner
├── grader.py               ← Scoring logic
└── requirements.txt        ← Add new packages here
```

---

## Common Issues

**`ModuleNotFoundError: No module named 'rasterio'`**
```bash
pip install rasterio
```

**`Port 7860 already in use`**
```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

**Docker build fails**
Make sure Docker Desktop is running, then retry.

**Chandrayaan-2 data won't download**
PRADAN portal sometimes needs institutional access for certain datasets. Try the public archive first, or ping Yashika.

---

## Need Help?

- Check `docs/PS08_BRIEF.md` for conceptual questions
- Check `docs/TEAM_TASKS.md` for task ownership
- Ping Yashika on the team group for anything else
