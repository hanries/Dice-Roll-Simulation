# Horse Race — 3 Strategies

An interactive visualization of the classic **"25 horses, 5 lanes"** puzzle, comparing three different strategies for finding the 3 fastest horses without a stopwatch.

---

## The puzzle

> There are 25 horses, each running at a unique constant speed. The track has only 5 lanes, so at most 5 horses can race at once. You cannot time the horses — you only know the **finishing order** within each race. What is the minimum number of races needed to identify the 3 fastest horses?

The answer is **7**, but *why* 7, and how does it compare to naive approaches? That's what this tool shows.

---

## Strategies

### Strategy 1 — Rolling top 3
Race 5 horses at a time. Keep the top 3 from each race, drop the bottom 2, add 2 new horses, and repeat until all 25 have competed.

- **Races required:** ~11 on average
- **Problem:** The top 3 from an early race must re-race every single subsequent round — even after their ranking is already settled. Lots of redundant information.

### Strategy 2 — Batch + bracket
Race 5 groups of 5 (races 1–5). Then run a positional bracket: race all 5 first-placers (race 6), all 5 second-placers (race 7), all 5 third-placers (race 8). The winners of races 7 and 8 join the top 3 from race 6 for a championship (race 9).

- **Races required:** Always 9
- **Problem:** The bracket structure doesn't exploit the information we already have from group races. Horses are seeded by position, not by how informative a race would be.

### Strategy 3 — Optimal (7 races)
Race 5 groups of 5 (races 1–5). Race the 5 group winners (race 6) — the overall fastest is confirmed immediately, and logical deduction eliminates most remaining horses. Only 5 candidates can possibly be 2nd or 3rd; race those 5 (race 7).

- **Races required:** Always **7** — proven optimal
- **Key insight:** After race 6, you don't need to re-race the champion. And horses like D2, E1, C2, C3, B3 etc. are *logically* eliminated — there are already 3 horses provably faster than them.

#### Why exactly these 5 candidates remain after race 6?

Say the race 6 result is A1 > B1 > C1 > D1 > E1 (where X1 = winner of group X).

| Candidate | Reason they're still in contention |
|-----------|-----------------------------------|
| A2 | Lost only to A1, the overall champion |
| A3 | Lost only to A1 and A2 |
| B1 | Lost only to A1 in race 6 |
| B2 | Lost only to B1, who lost only to A1 |
| C1 | Lost only to A1 and B1 in race 6 |

Everyone else has **at least 3 horses provably faster than them**, so they can't be in the top 3.

---

## How to use

1. Open `horse_race_simulation.html` in any modern browser — no server, no dependencies, no build step.
2. Pick a strategy tab.
3. Click **▶ Run** to watch the simulation unfold round by round.
4. Use the **Speed** slider to slow down or speed up the animation.
5. Click **Reset** to re-randomize horse speeds and run again.

Each round appears as a row in a vertical timeline. Horse chips are color-coded:

| Color | Meaning |
|-------|---------|
| 🟡 Gold | 1st place in this race |
| ⬜ Silver | 2nd place |
| 🟠 Bronze | 3rd place |
| 🟢 Green | Advances to the next round |
| ⬛ Gray (strikethrough) | Eliminated |

An explanation panel below each round describes what information was gained and which horses were eliminated — and why.

---

## File structure

```
horse_race_simulation.html   # The entire app — single self-contained file
README.md                    # This file
```

The simulation is a single HTML file with no external dependencies beyond Google Fonts (loaded via CDN for typography only). All logic, state, and rendering are plain JavaScript — no frameworks.

---

## Key properties of the optimal solution

- **7 is a lower bound** — you need at least 5 races just to see every horse once, and at least 1 more to compare group winners, and at least 1 more to resolve the final candidates. You cannot do it in 6.
- **7 is achievable** — strategy 3 always finishes in exactly 7 races, regardless of how horse speeds are distributed.
- **The champion is free** — after race 6, the fastest horse is known without any additional race. This is the key saving over strategy 2.
- **The deduction does the work** — races 1–6 generate enough *relational* information to prune the field from 24 candidates down to exactly 5. Race 7 then resolves those 5.

---

## Background

This is a classic computer science and mathematical puzzle, often asked in technical interviews. It's a clean example of **information-theoretic reasoning** — the question isn't just "how many races can we run?" but "what is the minimum number of races that *must* be run to guarantee certainty?"

The puzzle generalizes: with $n$ horses, $k$ lanes, and wanting the top $m$ — the optimal strategy becomes significantly more complex.
