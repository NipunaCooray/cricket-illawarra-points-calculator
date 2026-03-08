# Cricket Illawarra Points Calculator

A single-file web app for calculating and tracking Cricket Illawarra competition points.

## Usage

Open `index.html` in any modern browser. Match data is saved to `localStorage` — no server required.

---

## Points System

### Base Points

| Result | Points |
|--------|--------|
| Win | 6 |
| Tie | 4 |
| Loss | 0 |

### Bonus Points (1G and 2G grades only)

A team can earn **up to 2 bonus points** per match on top of their base points.

---

## Win Determination

### Normal Match (no DLS)

- **Team 1 wins** if `T1 runs > T2 runs`
- **Team 2 wins** if `T2 runs > T1 runs`
- **Tie** if `T1 runs == T2 runs`

### DLS — Concluded Match (Rule 4b)

Used when T2's innings is concluded after interruptions with a revised target.

- Enter the **DLS Target** (the revised runs T2 needs to win)
- **Team 2 wins** if `T2 runs >= DLS Target`
- **Team 1 wins** if `T2 runs < DLS Target`
- **Tie** if `T2 runs == DLS Target`

> Example: Helensburg scores 159 in 50 overs. Rain reduces Wests' target to 80 in 25 overs. Wests score 83 in 17.4 overs → Wests win (83 ≥ 80).

### DLS — Abandoned Match (Rule 4a)

Used when a match is abandoned mid-innings and the result is determined by Par Score.

- Enter the **DLS Par Score** (T2's par score at the point of abandonment)
- **Team 2 wins** if `T2 runs > Par Score`
- **Team 1 wins** if `T2 runs < Par Score`
- **Tie** if `T2 runs == Par Score`

---

## Bonus Point Rules

Bonus points apply to **1G and 2G grades only**. Each team can earn 0, 1, or 2 bonus points independently of the other team's result.

### Batting First Bonus Points

Bonus is awarded based on the **runs ratio**: `T1 runs ÷ T2 runs`

| Ratio | Bonus Points |
|-------|-------------|
| ≥ 1.50× | 2 BP |
| ≥ 1.25× | 1 BP |
| < 1.25× | 0 BP |

> Example: T1 scores 200, T2 scores 142 → ratio = 1.41 → **1 BP**
> Example: T1 scores 200, T2 scores 128 → ratio = 1.56 → **2 BP**

**DLS adjustment for batting first bonus:**

- *Concluded match*: effective T1 score = `DLS Target - 1` (the adjusted "equivalent" T1 score)
- *Abandoned match*: effective T1 score = `DLS Par Score`

### Batting Second Bonus Points

Bonus is awarded based on **how quickly T2 wins** (overs remaining when winning run is scored).

#### Full match (50 overs per side)

| Overs used to win | Bonus Points |
|-------------------|-------------|
| ≤ 33.2 overs | 2 BP |
| ≤ 40.0 overs | 1 BP |
| > 40.0 overs | 0 BP |

#### Reduced overs match

Thresholds scale proportionally with max overs:

| Overs used to win | Bonus Points |
|-------------------|-------------|
| ≤ 66.67% of max overs | 2 BP |
| ≤ 80% of max overs | 1 BP |
| > 80% of max overs | 0 BP |

> Example: 25-over match — 2 BP threshold = 16.4 overs, 1 BP threshold = 20.0 overs.

---

## Overs Notation

Overs are entered in cricket notation: `X.Y` where `Y` represents balls (0–5).

- `33.2` = 33 overs and 2 balls = 33.333 decimal overs
- `17.4` = 17 overs and 4 balls = 17.667 decimal overs

---

## Grades

Bonus points are only calculated for **1G** and **2G** grades. All other grades receive base points (win/loss/tie) only.
