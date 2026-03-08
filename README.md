# Cricket Illawarra Points Calculator

A single-file web app for calculating and tracking Cricket Illawarra competition points.

## Usage

Open `index.html` in any modern browser. Match data is saved to `localStorage` — no server required.

### Features

- **Add Match** tab — enter match details with a live points preview before saving
- **Ladder** tab — auto-generated standings sorted by points, then wins
- **History** tab — full match log with edit and remove actions
- Grade filter pills on Ladder and History when multiple grades are in use
- Export match data to JSON / Import from a previously exported file
- Team name autocomplete drawn from previously entered teams

---

## Grades

| Grade          | Overs | Bonus Points |
| -------------- | ----- | ------------ |
| 1G (1st Grade) | 50    | Yes          |
| 2G (2nd Grade) | 50    | Yes          |
| 3G (3rd Grade) | 45    | No           |
| 4G (4th Grade) | 45    | No           |

---

## Match Types

| Type      | Description                                             |
| --------- | ------------------------------------------------------- |
| Normal    | Both innings played; enter runs and overs for each team |
| Tie       | Match declared a tie before or without play             |
| Abandoned | Match abandoned mid-play; no result                     |
| Cancelled | Match cancelled; no result                              |
| Forfeit   | One team forfeits                                       |

---

## Points System

### Normal Match — Win/Loss

| Grade   | Winner        | Loser                 |
| ------- | ------------- | --------------------- |
| 1G / 2G | 6 + bonus pts | max(0, 2 − bonus) pts |
| 3G / 4G | 6 pts         | 2 pts                 |

### Other Results

| Result    | 1G / 2G                        | 3G / 4G    |
| --------- | ------------------------------ | ---------- |
| Tie       | 4 pts each                     | 4 pts each |
| Abandoned | 4 pts each                     | 3 pts each |
| Cancelled | 4 pts each                     | 3 pts each |
| Forfeit   | 6 pts (winner) / 2 pts (loser) | same       |

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

Bonus points apply to **1G and 2G grades only**. Each team can earn 0, 1, or 2 bonus points independently.

### Batting First Bonus Points

Bonus is awarded based on the **runs ratio**: `effective T1 runs ÷ T2 runs`

| Ratio   | Bonus Points |
| ------- | ------------ |
| ≥ 2.00× | 2 BP         |
| ≥ 1.50× | 1 BP         |
| < 1.50× | 0 BP         |

> Example: T1 scores 200, T2 scores 142 → ratio = 1.41 → **0 BP**
> Example: T1 scores 200, T2 scores 130 → ratio = 1.54 → **1 BP**
> Example: T1 scores 200, T2 scores 99 → ratio = 2.02 → **2 BP**

**DLS adjustment for batting first bonus:**

- *Concluded match* (Rule 4b): effective T1 score = `DLS Target − 1`
- *Abandoned match* (Rule 4a): effective T1 score = `DLS Par Score`

### Batting Second Bonus Points

Bonus is awarded based on **how quickly T2 wins** (overs used when the winning run is scored).

#### Standard match (50 overs per side)

| Overs used to win | Bonus Points |
| ----------------- | ------------ |
| ≤ 25.0 overs      | 2 BP         |
| ≤ 33.2 overs      | 1 BP         |
| > 33.2 overs      | 0 BP         |

#### Reduced overs match

Thresholds scale proportionally with max overs:

| Overs used to win     | Bonus Points |
| --------------------- | ------------ |
| ≤ 50% of max overs    | 2 BP         |
| ≤ 66.67% of max overs | 1 BP         |
| > 66.67% of max overs | 0 BP         |

> Example: 25-over match — 2 BP threshold = 12.3 overs, 1 BP threshold = 16.4 overs.

---

## Overs Notation

Overs are entered in cricket notation: `X.Y` where `Y` represents balls (0–5).

- `33.2` = 33 overs and 2 balls = 33.333 decimal overs
- `17.4` = 17 overs and 4 balls = 17.667 decimal overs
