# 2026 NFL Fantasy Football Rankings Project
 
Projecting 2026 PPR fantasy points per game for veteran (non-rookie) wide receivers and running backs, and combining those results with a separate rookie projection model into one overall ranking.
 
This is a companion project to [2026 Fantasy Football Rookie Rankings](https://github.com/kevinkietle/2026-Fantasy-Football-Rookie-Rankings) — Part 4 of this project pulls in the rookie outputs from that repo and merges them with the veteran projections built here.

The full memo with takeaways can be found [here](https://docs.google.com/document/d/1vysHy8IBaBV0NGA-7fWy-G571C4oE9f-/edit?usp=sharing&ouid=117233643725020281980&rtpof=true&sd=true).
 
## Project Structure
 
The project is organized as a four-part notebook pipeline, meant to be run in order:
 
1. **`1_data_scraping_and_cleaning.ipynb`** — Scrapes weekly player stats via `nflreadpy`, aggregates them into season-level totals, and builds the modeling datasets. This includes creating "teammate" and "quarterback" versions of the stat tables (since part of the model relies on a player's best teammate and QB's stats), joining in a manually-built (Claude-assisted, QA'd) dataset of the top 60 non-rookie WRs and RBs from 2021–2026 with their projected QB/teammate for the upcoming season, and cleaning up name-matching and null-imputation issues. Outputs a final cleaned dataset for each position.
2. **`2_wr_rankings.ipynb`** — Builds and compares WR projection models (linear regression, ridge regression, XGBoost, and an ensemble of ridge + XGBoost), then uses the best model to simulate and project 2026 WR fantasy points per game and rankings.
3. **`3_rb_rankings.ipynb`** — Same modeling approach as the WR notebook, applied to running backs.
4. **`4_total_rankings.ipynb`** — Combines the WR and RB veteran projections from notebooks 2 and 3 with the rookie projections pulled from the separate [rookie rankings project](https://github.com/kevinkietle/2026-Fantasy-Football-Rookie-Rankings) into one overall set of rankings.

## Methodology
 
For each position (WR, RB), the pipeline tries several modeling approaches on the same feature set and compares them on R² and mean absolute error (MAE):
 
- **Linear Regression** — a full-feature version (v1) and a narrowed-feature version (v2) to reduce overfitting
- **Ridge Regression** — regularized version of the winning linear model
- **XGBoost** — full-feature and narrowed-feature gradient-boosted tree versions
- **Ensemble** — a blend of the best-performing linear/ridge model and the best-performing XGBoost model, weighted based on relative performance
Key features include the player's previous season stats, age, games played, and the projected stats of their QB and best teammate for the upcoming season.
 
For 2026 projections, the ensemble model is also used to simulate outcomes and produce tier probabilities (e.g., a player's odds of finishing as a WR1 vs. WR2), not just a single point estimate.
 
## Setup
 
```bash
pip install -r requirements.txt
```
 
Then run the notebooks in order (1 → 2 → 3 → 4) using Jupyter:
 
```bash
jupyter notebook
```
 
## Data Sources
 
- Play-by-play and weekly stats via [`nflreadpy`](https://github.com/nflverse/nflreadpy)
- Manually compiled (Claude-assisted, QA'd) datasets of top-60 non-rookie WRs and RBs per season, including projected QB/teammate and age
- Rookie projections from the separate [2026 Fantasy Football Rookie Rankings](https://github.com/kevinkietle/2026-Fantasy-Football-Rookie-Rankings) project
 
## Author

Kevin Le

- Growth Marketing Analyst
- M.S. Data Science Candidate at UT Austin
- Aspiring Sports Data Scientist