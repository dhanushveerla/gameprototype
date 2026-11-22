# ⚡ Swarm Protocol // Overclock

> **A 2D Bullet-Heaven Arena Survival Game & Real-Time Browser Performance Benchmark**  
> Built purely with vanilla JavaScript and HTML5 Canvas in a single, self-contained file (`index.html`) — zero frameworks, zero external libraries, zero asset downloads.

---

## 🎮 What Is This Game About?

**Swarm Protocol** is an intense, fast-paced 2D top-down bullet-heaven survival game (in the style of *Vampire Survivors*). You pilot a glowing cyber-drone in a dark digital arena, auto-firing laser bolts at relentless geometric swarms. 

As time ticks forward:
- Enemies escalate every 10 seconds, scaling from **50 initial enemies** up to **1,500+ simultaneous enemies** and hundreds of piercing laser bolts within 2 minutes.
- Defeated enemies drop glowing XP shards. Collecting shards levels up your drone with rapid fire rates, wide multi-shot spreads, and multi-enemy piercing.
- **Goal:** Survive for **3 full minutes (180 seconds)** to achieve Victory!

---

## 🔬 What Does This Game Aim to Show? (The Real-Time Benchmark)

Have you ever wondered why some web games start lagging and stuttering the moment lots of enemies appear on screen?

**Swarm Protocol** was built with a built-in, live laboratory experiment: **The Live Optimization Switch (`O` Key)**.

With the tap of a single key, you can switch the underlying game engine mid-game between two completely different programming architectures **without resetting your game or altering the number of entities on screen**:

### 1. 🔴 UNOPTIMIZED Mode (`MODE: UNOPTIMIZED`)
*How many casual web games are coded:*
- Every enemy, bullet, and XP shard is an individual JavaScript object allocated on the heap (`new EnemyEntity()`, nested `{ x, y }` vectors).
- Checks collisions using naive nested loops and un-squared distance calculations (`Math.hypot` / square roots).
- Draws entities one by one using unbatched canvas calls (`ctx.save()`, `ctx.rotate()`, `ctx.shadowBlur`, `ctx.restore()`).
- High Garbage Collection (GC) churn from recreating and filtering arrays every frame.
- **Result:** As enemy counts climb past 500–1,000, your CPU work time spikes dramatically, causing noticeable frame drops and stuttering.

### 2. 🟢 OPTIMIZED Mode (`MODE: OPTIMIZED`)
*High-performance game engine engineering:*
- **Flat Typed Arrays (Structure of Arrays):** Pre-allocated memory buffers (`Float32Array`, `Int32Array`) with zero per-frame memory allocation.
- **Flat Spatial Hash Grid:** Divides the arena into spatial cells, allowing bullets to only check nearby enemies instead of scanning the entire swarm.
- **Squared Distance Checks:** Collision math avoids costly square root (`Math.sqrt`) operations everywhere possible.
- **Batched Canvas Rendering:** Precomputes geometry and renders thousands of entities in a tiny handful of batched draw calls with zero `shadowBlur` overhead.
- **Result:** Even with **1,500+ enemies, 600+ bullets, and 1,500+ XP shards** on screen simultaneously (over 3,500–4,000+ total active items), frame times remain smooth and CPU load stays in the low single-digit milliseconds.

---

## 👀 What You Can See With Your Own Eyes (The Top-Left HUD)

Look at the benchmark panel in the top-left corner during gameplay:

| Metric | What It Shows | What to Watch For |
| :--- | :--- | :--- |
| **FPS** | Frames Per Second | High and green (60 FPS) in Optimized mode; watch it drop into amber/red in Unoptimized mode as the swarm grows. |
| **Frame Time (ms)** | Total time to render one frame | ~16.7 ms = solid 60 FPS. Higher values indicate frame drops. |
| **CPU Time (ms)** | Actual time your CPU spent computing physics & drawing | In Optimized mode with 1,500+ enemies, CPU time is typically **~2 to 4 ms**. Press **`O`**, and watch it immediately jump to **~50 to 90+ ms**! |
| **Live Sparkline** | Real-time performance graph | Draws a green line when Optimized, and spikes to a jagged red trace when Unoptimized. |
| **Active Entities** | Total count on screen | Shows total items plus breakdown of **ENM** (enemies), **BLT** (bullets), and **XPD** (XP shards). |
| **Mode Button** | Current engine mode | Displays either `MODE: OPTIMIZED` (cyan/green) or `MODE: UNOPTIMIZED` (red). You can click it anytime or press **`O`**. |

---

## 🚀 How to Clone and Play

You do **not** need Node.js, npm, Webpack, or any compiler. All you need is a web browser (Google Chrome, Firefox, Chromium, Edge, or Brave).

### Step 1: Clone the Repository
Open your terminal and run:
```bash
git clone https://github.com/<YOUR-USERNAME>/<YOUR-REPO-NAME>.git
cd <YOUR-REPO-NAME>
```

### Step 2: Run & Open in Browser

Choose **any** of the following options:

#### Option A: Direct File Open (Easiest)
Simply double-click `index.html` in your file explorer, or open it directly in your browser:
- **Linux:**
  ```bash
  google-chrome index.html
  # or
  firefox index.html
  ```
- **macOS:**
  ```bash
  open index.html
  ```
- **Windows:**
  ```cmd
  start index.html
  ```

#### Option B: Simple Local Web Server (Recommended)
If you prefer running a local server:
```bash
# Using Python 3:
python3 -m http.server 8080

# Using Node.js (if installed):
npx serve .
```
Then open your browser and navigate to:
```
http://localhost:8080
```

---

## 🕹️ Controls

| Key | Action |
| :--- | :--- |
| **WASD** or **Arrow Keys** | Move your drone in 8 directions (also starts the game) |
| **Auto-Fire** | Your drone automatically locks onto and shoots the nearest targets |
| **O** | **Toggle Optimization Mode live mid-game** (Optimized ↔ Unoptimized) |
| **P** or **Escape** | Pause / Resume the game |
| **M** | Mute / Unmute procedural sound effects |
| **R** | Instant restart run |

---

## 🧪 Try This Experiment!

1. Start the game by pressing **Space** or any **WASD** key.
2. Kite enemies in wide circles, collecting glowing XP shards to level up your weapon into a multi-shot bullet storm.
3. Wait until around **1:30 – 2:00** on the timer, when the wave surges past **1,000 to 1,500 active enemies**.
4. Check your HUD: note the **CPU ms** (usually 2–4 ms) and solid **FPS**.
5. Now press **`O`** to switch to **UNOPTIMIZED** mode:
   - Notice the exact same enemies and bullets remain on screen.
   - Watch the HUD: **CPU Time** will skyrocket and FPS will take a heavy hit.
6. Press **`O`** again to return to **OPTIMIZED** mode:
   - Everything instantly recovers to smooth performance.

---

## 🛠️ Tech Stack & Design
- **Single File:** Everything (HTML, CSS layout, Canvas 2D engine, audio synthesizer) is contained within `index.html`.
- **Zero Assets:** All geometry (drone, swarmers, tanks, shards) is procedurally drawn via Canvas vector paths.
- **Audio:** Real-time procedural audio synthesis using the Web Audio API (OscillatorNode & GainNode) — no audio files to load.
- **Platform:** Tested and running smoothly on Linux, macOS, and Windows on both low-end and high-end hardware.

---


