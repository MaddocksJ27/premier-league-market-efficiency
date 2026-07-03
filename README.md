# Premier League Betting Market Efficiency Study

This project tests how difficult it is to beat Premier League bookmaker opening odds.

It is framed as a **market efficiency analysis**, not a guaranteed profitable betting model. The notebook compares bookmaker implied probabilities against several simple football models, then checks whether any model-selected bets produce profit or closing-line value on a true out-of-sample test season.

## Project Question

Can pre-match football features improve on Bet365 opening odds for Premier League match outcomes?

The analysis focuses on:

- Opening-odds calibration
- Model probability quality
- Log loss and Brier score versus the market
- Betting ROI under simple staking assumptions
- Closing-line value on selected bets
- Where model predictions disagree with the market

## Data

Place CSV files in:

```text
data/PremCSV/
```

The project currently supports two data formats:

- Historical PremCSV files, such as `england-premier-league-matches-2021-to-2022-stats.csv`
- Football-data style `E0 (1).csv` for the 2025-2026 test season

For `E0 (1).csv`, the notebook uses:

- Opening odds: `B365H`, `B365D`, `B365A`
- Closing odds: `B365CH`, `B365CD`, `B365CA`

## Method

The notebook:

1. Loads and combines Premier League CSV files.
2. Normalizes both PremCSV and football-data schemas.
3. Builds market-implied probabilities from opening odds.
4. Creates pre-match features such as rolling form, goals, xG, shots, cards, rest days, fixture congestion, promoted-team flags, and Elo ratings.
5. Trains on historical seasons before 2025-2026.
6. Tests on `E0 (1).csv` as the 2025-2026 out-of-sample season.
7. Compares model variants against the opening market.
8. Backtests model-selected edges using flat stakes and commission.
9. Measures closing-line value using Bet365 closing odds.

## Model Variants

The portfolio comparison includes:

- Market only
- Team features only
- Elo only
- Market plus team features
- Regularized market plus team features
- Tuned market/model blend
- Calibrated regularized logistic regression
- Calibrated gradient boosting

The notebook also compares proportional overround removal against power and Shin-style de-vigging.

## Headline Results

Out-of-sample test season: **2025-2026** (`E0 (1).csv`)  
Test matches after odds filtering: **339**

| Model | Log loss | Log loss vs opening market | Accuracy | Brier score |
| --- | ---: | ---: | ---: | ---: |
| Calibrated regularized LR | 1.257 | -0.091 | 0.490 | 0.619 |
| Calibrated gradient boosting | 1.326 | -0.022 | 0.481 | 0.615 |
| Team features only | 1.332 | -0.015 | 0.442 | 0.658 |
| Elo only | 1.342 | -0.006 | 0.478 | 0.623 |
| Opening market | 1.348 | 0.000 | 0.496 | 0.610 |

The best probability model improves log loss versus the opening market, but the betting evidence is weaker. The best short-run flat-staking ROI came from the Elo-only model, but its average closing-line value was still negative:

| Betting diagnostic | Value |
| --- | ---: |
| Best flat-staking ROI model | Elo only |
| Bets placed | 266 |
| ROI on staked | 2.36% |
| Average CLV on bets | -0.72% |
| Bootstrap CLV 95% CI | [-1.76%, 0.35%] |
| Bootstrap CLV p-value vs zero | 0.179 |

That is the central market-efficiency result: the model can find attractive-looking historical edges, but there is not strong evidence that those bets beat the closing line.

![Calibration plot](reports/figures/calibration_plot.png)

## Key Takeaway

The project shows that Premier League opening odds are a very strong benchmark. Some model variants may improve individual probability metrics or short-run ROI, but the important test is whether those bets also earn positive closing-line value.

The current conclusion is intentionally conservative: there is no robust evidence of a durable betting edge unless a model beats the opening market and consistently beats the closing line.

## How To Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Open and run:

```text
premier_league_market_efficiency_model.ipynb
```

The committed notebook is executed end to end so GitHub renders the results and charts without needing to launch Jupyter.

## Requirements

- Python
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- jupyter

## Notes

This is a research and portfolio project. It is not financial advice and should not be treated as a betting recommendation system.
