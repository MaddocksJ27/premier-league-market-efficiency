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

The executed copy is:

```text
premier_league_market_efficiency_model_executed.ipynb
```

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
