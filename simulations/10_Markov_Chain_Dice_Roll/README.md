# Monte Carlo Simulation — Rolls Until |a − b| = 2

A self-contained HTML visualization built for the *hanrius* YouTube channel.

## The Problem

Roll a fair 6-sided die repeatedly. Stop when two consecutive rolls differ by exactly 2. How many rolls do you expect to make?

**Exact answer: 17/3 ≈ 5.667**

Derived using a Markov chain with two transient states:
- **S₁** — last roll ∈ {1, 2, 5, 6} → 1 winning neighbor → P(win) = 1/6
- **S₂** — last roll ∈ {3, 4} → 2 winning neighbors → P(win) = 2/6

Setting up and solving the system of equations:

```
E₁ = 1 + (4/6)·E₁ + (1/6)·E₂     →     E₁ = 5
E₂ = 1 + (2/6)·E₁ + (2/6)·E₂     →     E₂ = 4

E₀ = 1 + (4/6)·E₁ + (2/6)·E₂
   = 1 + (4/6)·5  + (2/6)·4
   = 17/3 ≈ 5.667
```

## File

`monte_carlo_die.html` — single self-contained file, no build step, no dependencies beyond a CDN load of Chart.js.

## Usage

Open `monte_carlo_die.html` in any modern browser. No server required — just double-click the file.

### Run simulation
Select a trial count from the dropdown (500 to 100,000) and click **Run**. The distribution chart updates and accumulates across multiple runs — you can keep clicking Run to add more trials without resetting.

### Watch one trial
Click **Watch one trial** to animate a single game step by step. Each roll appears as a chip. The winning pair is highlighted in coral when the game ends.

### Reset
Clears all accumulated data and resets the chart.

## What to look for

- The distribution is right-skewed — most games finish in 2–6 rolls, but the tail extends well beyond that.
- The red dashed line marks the exact mean at 17/3. With enough trials the simulated mean converges toward it.
- The error shown in the stat card shrinks as trial count increases, demonstrating the law of large numbers.

## Dependencies

- [Chart.js 4.4.1](https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js) — loaded from CDN, requires an internet connection on first open.

## Notes

- The simulation accumulates across runs within the same session. Refreshing the page resets everything.
- Tested in Chrome, Firefox, and Safari.
