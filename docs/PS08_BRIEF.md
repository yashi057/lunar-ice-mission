# 📖 PS 08 — Complete Brief for Team Members

> **Read this first if you just joined the team.** This explains the entire problem statement from zero — no prior knowledge needed.

---

## Part 1 — What is Reinforcement Learning (RL)?

Our project is built on RL, so you need to understand this first.

**The dog-training analogy:**

Imagine training a dog. You say "sit." If the dog sits → you give it a biscuit (reward). If it runs off → no biscuit. After many tries, the dog learns by trial and error: *"sitting = biscuit."*

Same principle in our code:
- **Dog** = the drone / rover AI
- **Biscuit** = positive reward number
- **Trainer** = the reward function (a table of +2 / +10 / -15 / +20 values)

The AI (called the **agent**) starts knowing nothing. It tries random moves, gets feedback (reward or penalty), and slowly learns what works.

### The RL Loop

```
Environment ──── Observation (position, battery, targets) ────▶ AI Agent
     ▲                                                               │
     └──────────── Action (UP / DOWN / LEFT / RIGHT) ───────────────┘
                        + Reward signal after each step
```

This loop repeats 50–150 times per episode. After thousands of episodes, the agent figures out the best strategy.

---

## Part 2 — What is AstroNav? (Our existing project)

AstroNav is a 2D grid-based RL environment simulating Mars drone navigation.

```
. . T . . . . . . .
. X . . . . X . . .
. . . . X . . . . .
B . . . . . . D . .
. . X . . T . . . .
```

| Symbol | Meaning |
|--------|---------|
| `B` | Base station — drone must return here |
| `D` | Drone — the AI agent |
| `T` | Geological target — worth +10 reward |
| `X` | Obstacle — -15 penalty if hit |
| `.` | Unknown terrain — +2 reward when explored |

**Reward function (the "teacher"):**

| Event | Reward | Why |
|-------|--------|-----|
| New cell explored | +2 | Encourages exploration |
| Target found | +10 | Main mission objective |
| Obstacle collision | -15 | Strongly discourages crashing |
| Each step | -0.1 | Battery drains — don't wander forever |
| Safe return to base | +20 | Biggest reward — complete the mission |
| Battery depleted | -20 | Failure |

**Benchmark results:**

| Difficulty | Grid | Score |
|------------|------|-------|
| Easy | 6×6 | 0.55 ✅ |
| Medium | 10×10 | 0.41 ✅ |
| Hard | 15×15 | 0.35 ✅ |

All three pass their respective thresholds. This is our foundation.

---

## Part 3 — What is PS 08?

**Full title:** *"Detection and Characterization of Subsurface Ice in Lunar South Polar Regions Using Chandrayaan-2 Radar and Imagery Data for Landing Site and Rover Traverse Planning"*

Let's break every piece down:

### Lunar South Polar Region

The Moon's south pole. Some craters here are permanently in shadow — sunlight never reaches them. This means temperatures can drop to -230°C. Scientists believe water ice survives underground in these permanently shadowed regions (PSRs). This is exactly where Chandrayaan-3 landed in August 2023 and made history.

### Subsurface Ice

Ice that is NOT on the surface — it's buried a few metres underground. You can't see it with a camera. You need a sensor that can "see through" the ground, the way an X-ray sees through skin.

### Chandrayaan-2 DFSAR (the radar)

India's Chandrayaan-2 satellite (launched 2019, still orbiting the Moon today!) carries a **Dual Frequency Synthetic Aperture Radar (DFSAR)**. It fires radar waves at the Moon's surface, the waves bounce back, and scientists can analyze those bounce patterns to infer what's underground.

**Key concept — CPR (Circular Polarization Ratio):**
When radar waves bounce off ice, they behave differently than when they bounce off rock. The ratio of these bounce signals (called CPR — Circular Polarization Ratio) is a reliable indicator of subsurface ice. High CPR value in a cell → probable ice.

### OHRC (the camera)

Chandrayaan-2 also has an **Optical High Resolution Camera (OHRC)** — basically a very good satellite photo camera. This gives us visual maps of craters, slopes, and permanently shadowed areas.

### Landing Site Planning

If we're sending a future rover (Chandrayaan-4 or beyond), WHERE should it land? The landing site needs to be:
- Flat enough for safe landing
- Close enough to ice deposits to be scientifically useful
- Not inside a permanent shadow (rover needs solar power)
- Away from dangerous craters

### Rover Traverse Planning

Once the rover lands, what PATH should it drive? The rover needs to:
- Reach ice-probable sites
- Avoid craters and steep slopes
- Manage its battery / solar power
- Return safely to its base station

---

## Part 4 — How AstroNav solves PS 08

This is our competitive advantage. Every piece of PS 08 maps directly onto something we already built.

| PS 08 requirement | AstroNav equivalent | What we change |
|-------------------|--------------------|----|
| Lunar terrain grid | 2D Mars grid | Replace random grid with Chandrayaan-2 data |
| Ice deposit location | Geological target `T` | Ice-probable cells become targets |
| Craters + PSRs | Obstacle `X` | Hazard cells from OHRC + DFSAR |
| Rover battery/solar | Drone battery | Same logic, different label |
| Landing site | Base station `B` | Candidate landing sites |
| Traverse path | Agent's path | Output of RL agent |

**What we do new:**
1. Download Chandrayaan-2 data from ISRO PRADAN portal
2. Run CPR analysis on DFSAR to find ice-probable zones
3. Use OHRC imagery to flag craters and PSRs as hazards
4. Convert this into a grid (same format as AstroNav's environment)
5. Feed the grid into our RL agent
6. Collect output: best traverse path + ranked landing sites

---

## Part 5 — What data is available?

All data is **free and publicly accessible** from ISRO's PRADAN portal:
- 🔗 [https://pradan.issdc.gov.in/](https://pradan.issdc.gov.in/)
- Instrument: `CH2-Dual Frequency SAR` for DFSAR data
- Instrument: `CH2-OHRC` for optical imagery
- Target region: Lunar South Pole (`lat: -89° to -85°`)

Data format: GeoTIFF / HDF5 files. We read these using `rasterio`.

---

## Part 6 — Hardware knowledge — do you need any?

**Short answer: NO.** Not for this hackathon.

PS 08 is a purely data-and-software problem. The hardware (Chandrayaan-2 satellite) is already in orbit doing its job. We just download and process the data it collected.

Useful (NOT mandatory) background:
- Rough idea of how SAR radar returns work (Google "SAR remote sensing basics" — 10 min read)
- What permanently shadowed regions are (YouTube: "Moon's south pole ice" — 5 min)

None of this requires owning, building, or operating any physical equipment.

---

## Summary — What you need to remember

1. **AstroNav** = our existing Mars RL navigation AI. It already works. We are adapting it, not replacing it.
2. **PS 08** = use Chandrayaan-2 satellite data to find underground ice and plan rover routes on the Moon's south pole.
3. **Our idea** = replace AstroNav's random grid with real Chandrayaan-2 data, and the agent's "geological targets" with ice-probable cells.
4. **No hardware needed.** Pure software + open ISRO data.
5. **Start with `SETUP.md`** to get the project running locally.

---

*Questions? Ping Yashika on the team group.*
