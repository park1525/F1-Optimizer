# 🏎️ F1 Fantasy Team Optimizer

A web-based tool that analyzes F1 practice session lap times to find the optimal driver and constructor combination within your budget cap.

🔗 **[Live Site](https://park1525.github.io/F1-Optimizer)**

---

## 📋 How It Works

1. Prepare your Excel file following the format below
2. Upload the file to the site
3. Set your budget cap
4. Hit **Optimize** — the best team is calculated instantly

---

## 📊 Excel File Format

Your Excel file must have **at least 3 sheets** structured as follows:

---

### Sheet 1 — `Base`
> Driver roster that stays the same all season

| Name | Team |
|------|------|
| Verstappen | Red Bull |
| Norris | McLaren |
| Leclerc | Ferrari |
| ... | ... |

---

### Sheet 2 — `Team Price`
> Constructor prices (update each race weekend)

| Team | Price |
|------|-------|
| Red Bull | 28.2 |
| McLaren | 27.5 |
| Ferrari | 23.3 |
| ... | ... |

---

### Sheet 3+ — `Grand Prix Name` (e.g. `Australia`, `Japan`)
> Add a new sheet for each Grand Prix weekend

| Name | Price | P1 | P2 | P3 |
|------|-------|----|----|-----|
| Leclerc | 22.8 | 1:20.267 | 1:19.991 | 1:20.105 |
| Hamilton | 22.5 | 1:20.736 | 1:20.050 | 1:21.003 |
| Verstappen | 27.7 | 1:20.789 | 1:20.366 | 1:20.512 |
| ... | ... | ... | ... | ... |

> ⚠️ **The last sheet is always used for optimization.** Add new Grand Prix sheets as the season progresses — the tool will automatically use the most recent one.

---

## ✅ Rules & Tips

**Lap time format**
```
1:23.456   ✅ correct
1:23:456   ❌ wrong (colon instead of period)
83.456     ❌ wrong (missing minutes)
```

**Didn't participate in a session?**
- Leave the cell **blank** — it will be treated as a non-participation automatically

**Driver names must match exactly across sheets**
- `Verstappen` in Sheet 1 must be spelled the same in Sheet 3+
- Capitalization matters

**Prices**
- Enter as numbers only, no `$` or `M` (e.g. `22.8` not `$22.8M`)

---

## 🧮 Scoring System

The scoring system is inspired by **F1 Qualifying session cutoffs** (Q1 / Q2 / Q3), where the gaps between groups get progressively larger as positions drop — just like how Q3 drivers are separated by smaller margins than Q1 eliminees.

Points are awarded based on each driver's lap time ranking per session:

| Group | Positions | Gap per position |
|-------|-----------|-----------------|
| 🟢 Q3 equivalent | 1st → 10th | −2 pts |
| 🟡 Q2 equivalent | 11th → 16th | −3 pts |
| 🔴 Q1 equivalent | 17th → 22nd | −4 pts |

| Position | Points | | Position | Points |
|----------|--------|-|----------|--------|
| 1st | 100 | | 12th | 76 |
| 2nd | 98 | | 13th | 73 |
| 3rd | 96 | | 14th | 70 |
| 4th | 94 | | 15th | 67 |
| 5th | 92 | | 16th | 64 |
| 6th | 90 | | 17th | 60 |
| 7th | 88 | | 18th | 56 |
| 8th | 86 | | 19th | 52 |
| 9th | 84 | | 20th | 48 |
| 10th | 82 | | 21st | 44 |
| 11th | 79 | | 22nd | 40 |
| DNS/DNP | 0 | | | |

> 💡 Want to tweak the scoring? Just update the `SCORE_TABLE` object in `index.html` — it's clearly labeled and easy to find.

A driver's final score = **average of their scores** across all sessions they participated in.

Constructor score = **sum of their two drivers' scores**.

---

## 💰 Optimization Logic

The optimizer tries every valid combination of:
- **5 drivers** from the full roster
- **2 constructors** from the constructor list

...and finds the combination with the **highest total score** that fits within your budget cap.

---

## 📁 Example File Structure

```
F1_Data.xlsx
├── Base          → Full driver roster + teams
├── Team Price    → Constructor prices
├── Australia     → Round 1 data
├── Japan         → Round 2 data
└── Bahrain       → Round 3 data  ← this one is used
```
