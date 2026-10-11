# Assignment 1 Report

- **Name**: Roberto Falcato
- **Student ID**: 20219
- **Email**: rfalcato.ieu2022@student.ie.edu
- **Group**: BBADBA 5B

## Dataset

The data come from the Ergast-based Formula 1 World Championship dataset (Kaggle), a relational set of 14 tables. I used seven: results (base table), races, drivers, constructors, circuits, qualifying, and status. One row represents one driver in one Grand Prix. After restricting to seasons 2014–2024 (hybrid-engine era), the modelling table has 4,626 rows and 13 model features. Post-race tables (lap_times, pit_stops) and post-race standings were excluded to avoid leakage and to respect the repository size limit. The dataset was chosen because the target must be built from several tables and because the temporal structure forces careful split and feature design.

## Business / real-life framing

FantasyGP, a fantasy F1 platform, wants to recommend drivers likely to score points (top-10 finish) in the upcoming race. The model runs after qualifying and before the race, so only the starting grid, qualifying results, and earlier races can be used. This determines three pipeline decisions:

Target: scores_points = 1 if points > 0. In 2014–2024, this is identical to finishing in the top ten (zero mismatches), so the target is unambiguous.
Split: chronological, because a random split would let future races inform predictions about past ones. Train 2014–2019 (2,487 rows, 48.7% positive), validation 2020–2021 (780 rows, 50.0%), test 2022–2024 (1,359 rows, 50.0%).
Threshold metric: expected misclassification cost. A false positive (recommending a non-scorer) costs 1; a false negative (missing a scorer) costs 2, since the platform’s value lies in finding undervalued drivers. The threshold is chosen on the validation set by minimising this cost, not fixed at 0.5. With calibrated probabilities, the theoretical cut-off is 1/(1+2) ≈ 0.33.

## Data preparation & feature engineering

*What you engineered and why, and any data-quality decisions you made along
the way — e.g. "segment X had defective data, so I excluded it and used a
population-average default for scope Y at inference time; the impact of
that choice is Z."*

## Modeling: three implementations, one model

*Which model (linear or logistic regression) and why. A results table
comparing scikit-learn, the manual PyTorch loop, and the standard
torch.nn.Module/torch.optim workflow, on the same test set, against the
naive baseline. Do the three agree? If not, why not?*

## Limitations & next steps

*Real limitations you found, and concretely how you'd address each one with
more time or data — not generic hedging.*

## Generative AI use disclosure

*Per the syllabus AI Policy: what you used and how, or "no AI content used."*
