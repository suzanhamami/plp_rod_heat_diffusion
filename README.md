# 1D Rod Heat Diffusion

A small C++ / OpenMP project that simulates a 1D diffusion (stencil) update over time and compares **sequential** vs **parallel** execution.

The program:
- Initializes a 1D array with a single heat “spike” in the center
- Runs `T` time steps of the update rule  
  `U_new[i] = 0.5 * (U[i-1] + U[i+1])`
- Writes **CSV snapshots** every 50 steps for:
  - sequential execution (`snapshots_seq.csv`)
  - parallel execution using OpenMP with 1, 2, 4, and 8 threads (`snapshots_1.csv`, `snapshots_2.csv`, `snapshots_4.csv`, `snapshots_8.csv`)
- Measures runtime and prints **speedup** and **efficiency** for each thread count

A Python script is included to visualize the CSV snapshots as an animation.

---

## Repository contents

- `PLPP1.cpp` — main simulation (sequential + OpenMP parallel, CSV snapshots, timing)
- `PLP_PROJECT1.sln`, `PLP_PROJECT1.vcxproj*` — Visual Studio project files
- `snapshots_*.csv`, `snapshots_seq.csv` — example output snapshots (may be regenerated)
- `visualization.py` — simple Matplotlib animation for a selected snapshots file

---

## Requirements

### C++ build
- A C++ compiler with OpenMP support:
  - Windows: Visual Studio (MSVC) with OpenMP enabled
  - Linux/macOS: GCC/Clang with OpenMP installed/enabled

### Python visualization
- Python 3
- `numpy`
- `matplotlib`

Install Python deps:
```bash
pip install numpy matplotlib
```

---

## Build & run

### Option A: Visual Studio (recommended for this repo setup)
1. Open `PLP_PROJECT1.sln`
2. Make sure OpenMP is enabled in project settings (C/C++ → Language → OpenMP Support)
3. Build and run

### Option B: Command line (example)

#### Linux (GCC)
```bash
g++ -O2 -fopenmp PLPP1.cpp -o plp1
./plp1
```

> Note: the code currently calls `system("pause");` at the end, which is Windows-specific.
> If you build on Linux/macOS, you may want to remove/comment that line.

---

## Output

When you run the program it generates:

- `snapshots_seq.csv` (sequential)
- `snapshots_1.csv`
- `snapshots_2.csv`
- `snapshots_4.csv`
- `snapshots_8.csv`

Each row in a snapshots file is:
- first column: the time step `t`
- remaining columns: the 1D array values at that time step

Snapshots are written at:
- `t = 0`
- every 50 steps (50, 100, …, 500)

The program also prints timing + speedup/efficiency to the console.

---

## Visualize results (Python)

By default, `visualization.py` loads `snapshots_8.csv`. To visualize a different run, change this line:

```python
data = np.genfromtxt("snapshots_8.csv", delimiter=",")
```

Run:
```bash
python visualization.py
```

A window will open showing an animated line plot of the array values over time.

---

## Notes / Implementation details

- Array size: `N = 500`
- Time steps: `T = 500`
- Parallel version uses OpenMP and a shared `(current, next)` pointer swap each time step.
- Snapshots are recorded inside a `#pragma omp single` section to avoid multiple threads writing simultaneously.

---

## Authors
* Louay Arnaba Khordaji
* Suzan Hamami
## License

Add a license if needed (e.g., MIT). If this is coursework, you may want to include a brief academic integrity note.
