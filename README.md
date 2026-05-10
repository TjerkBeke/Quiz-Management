# 🎯 Quiz Scoring Workbook

A self-contained Excel scoring system for live pub quizzes, classroom quizzes, or any multi-round multi-team competition. Enter scores once on the **Dashboard** and the **Standings** ranking and **Statistics** dashboard update automatically.

Supports up to **30 rounds** and **60 teams** with custom round names, custom max points per round, a built-in tie-break mechanism, and a podium-styled final standings.

> ⚠️ **Compatibility note:** This workbook has only been tested on **Excel for Windows (Microsoft 365), build 16.0.19929.20136**. It is not guaranteed to work correctly on other Excel versions (older desktop releases, Excel for Mac, Excel on the web, or mobile). Some formulas, formatting, or features may behave differently or not at all.

---

## 📑 Sheets

The workbook has four tabs, each with a clear role:

| Tab | Purpose |
|---|---|
| **Management** | One-time setup. Define rounds, max points, round names, team names. |
| **Dashboard** | Score entry. Type each team's score per round here. |
| **Standings** | Auto-ranked final result. Gold/silver/bronze podium + full leaderboard. |
| **Statistics** | Auto-calculated insights — best score, field average, per-round difficulty, % perfect rounds. |

---

## 🚀 Quick start

1. Open the **Management** tab.
2. Set the **total number of rounds** in the peach box (cell `C4`) — max 30.
3. (Optional) Edit **max points per round** in column B.
4. (Optional) **Rename rounds** in column C (e.g. "Actua", "Doe-ronde", "Tussenronde").
5. Add or replace **team names** in column F — max 60 teams.
6. Switch to the **Dashboard** tab and enter each team's score per round.
7. Open **Standings** and **Statistics** to see live results.

That's it. Don't touch the formulas in any other column — they handle the rest.

---

## 🛠️ Management tab — setup

This is the only tab you configure manually before the quiz.

### Rounds (columns A–C)

- **`C4` — Total rounds** (peach box). Set this to however many rounds your quiz has. Everything downstream respects this number — extra rounds beyond `C4` are hidden from Dashboard headers, Standings, and Statistics.
- **Column A** — round numbers (auto, 1 to `C4`).
- **Column B — PTS** — max points awarded for each round. Defaults to 10. Override per round (e.g. 20 for a double round, 5 for a quick round).
- **Column C — NAME** — round names. If you leave it as the default `Round N`, headers display as `RN`. If you name it (e.g. `Actua`), the first 7 characters appear as the column header on Dashboard and Standings.

### Teams (column F)

- Type team names in **`F7:F66`**. Up to 60 teams.
- Empty cells are skipped automatically — only filled rows show up on the Dashboard and in the Standings ranking.

### How it works panel (column H)

The right-hand card on this tab is a permanent reminder of the workflow plus the tie-break explanation. Read it once.

---

## 📊 Dashboard tab — score entry

This is where you type during the quiz.

| Column | What goes there |
|---|---|
| **A — Team** | Auto-filled from Management column F. Don't type here. |
| **B — Tie-break** | Manual. See [Tie-break](#-tie-break) below. |
| **C–AF — R1…R30** | Manual. Each team's score for that round. Headers show round name and max (e.g. `Actua (/10)`). Only the first `C4` columns are active. |
| **AG — Total** | Auto: `=SUM(C:AF)`. |
| **AH — Max** | Auto: total possible points across all configured rounds. |
| **AI — Sort key** | Auto. Internal column used by Standings to break ties. Don't touch. |

**Rules of thumb:**
- Enter scores as plain numbers (e.g. `7`, not `7/10`).
- Don't exceed the round's max — the workbook won't stop you, but it'll throw off the % perfect statistic.
- Leave a cell blank if a team didn't participate in that round; it counts as zero.

---

## 🥇 Tie-break

The tie-break column (Dashboard column B) decides ranking when two or more teams finish on the same total score.

**How to use it:**
1. At the end of the quiz, ask one tie-break question with a numeric answer (e.g. *"How many islands does Greece have?"*).
2. Each team writes down a guess.
3. For each team, enter the **absolute difference** between their guess and the correct answer in column B.
4. **Lower = better.** The team closest to the correct answer wins the tie.

Example: correct answer is 6,000 islands. Team A guesses 5,200 → enter `800`. Team B guesses 7,500 → enter `1500`. Team A wins the tie.

If you don't use a tie-break, leave column B blank — ties will just resolve in row order.

**Behind the scenes:** the Standings ranking sorts on `Total − Tie-break/100000`, so a smaller tie-break value floats a team above its tied rivals without affecting the displayed score.

---

## 🏆 Standings tab — final results

Auto-generated from Dashboard. Sorted by total score (with tie-break applied).

### Podium styling

| Rank | Color | Treatment |
|---|---|---|
| 🥇 1st | Gold (`#FEF3C7`) | Largest font, taller row |
| 🥈 2nd | Silver (`#E5E7EB`) | Slightly smaller |
| 🥉 3rd | Bronze (`#FED7AA`) | Smaller still |
| 4th–60th | Cream/white bands | Compact list |

### Columns

- **A — Rank** (1, 2, 3, …)
- **B — Team** name
- **C — Score** in `points / max` format (e.g. `94 / 155`)
- **D–AG — per-round breakdown**, also `points / max` (e.g. `7/10`)

The first three columns are frozen so the team name and total stay visible while you scroll across rounds.

---

## 📈 Statistics tab — insights

Auto-computed metrics for post-quiz review or live commentary.

### Top section
- **Best score % of max** — the leading team's total as a percentage of the maximum possible.
- **Field average % of max** — average across all teams as a percentage of max.

### Round Difficulty table (columns V–AA)

For each round (rows 8–37):

| Column | Meaning |
|---|---|
| **V — Rd** | Round label (`R1`, `R2`, …) |
| **W — Avg** | Average score across all teams |
| **X — Max** | Highest score in that round |
| **Y — Min** | Lowest score in that round |
| **Z — Difficulty** | Bar chart of `█` characters. **Length = % of max × 20**, so 20 chars = everyone scored full marks, 0 chars = everyone got zero. Comparable across rounds regardless of point value. |
| **AA — % Perfect** | Percentage of teams that scored the round's max. |

The difficulty bar normalizes to percentage of max, so a round worth 20 points and a round worth 5 points can be compared at a glance — longer bar = easier round.

---

## ⚙️ How resizing works

The workbook is built to scale dynamically:

- Rounds beyond `Management!C4` are hidden from all headers and ranking outputs (formulas return blank).
- Teams beyond the last filled row in `Management!F7:F66` don't appear in the Standings.
- All formulas reference the configured ranges (`F7:F66`, `B7:B66`, etc.), so adding a 31st team or changing round count to 8 just works.

> ⚠️ **Excel version compatibility:** The dynamic resizing logic — and the workbook in general — has only been tested on **Excel for Windows (Microsoft 365), build 16.0.19929.20136**. Behavior on other Excel versions (older desktop releases, Excel for Mac, Excel on the web, or mobile) is not guaranteed.

---

## 🎨 Color coding

| Color | Meaning |
|---|---|
| Orange / peach (`#9A3412`, `#FFEDD5`) | Headers and accent boxes |
| Cream (`#FFF8F1`) | Banded row tint |
| Gold / silver / bronze | Podium rows on Standings |
| Blue text | Manual input cells (Dashboard scores, tie-breaks) |
| Black text | Formula-driven cells |

---

## 📁 File structure

```
quiz.xlsx
├── Management   ← setup (rounds, teams, max points)
├── Dashboard    ← score entry (60 teams × 30 rounds)
├── Standings    ← auto-ranked leaderboard with podium
└── Statistics   ← auto-computed insights and difficulty
```

---

## 💡 Tips

- **Test before the quiz.** Type a few sample scores, check that Standings updates, then clear them.
- **Don't sort the Dashboard manually** — Standings does the sorting for you. Sorting Dashboard breaks the team-row order and may corrupt formulas.
- **Custom round names** appear as the first 7 characters in headers. Pick names that read well truncated (e.g. `Actua`, `Lokaal`, `Hyperro`) or just leave the default `Round N`.
- **Tie-break can be any numeric measure** — distance to a correct number, time to complete a task, count of correct sub-answers — as long as lower = better.

---

## 📜 License

Free to use and modify. No warranty.
