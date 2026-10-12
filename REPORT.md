# Assignment 1 Report

- **Name**: Roberto Falcato
- **Student ID**: 20219
- **Email**: rfalcato.ieu2022@student.ie.edu
- **Group**: BBADBA 5B

## Dataset

The data is from the Formula 1 World Championship data set based on Ergast (Kaggle), a relational set of 14 tables. I used seven: results (base table), races, drivers, constructors, circuits, qualifying, status. Each row represents a driver in a single Grand Prix. After limiting to seasons 2014-2024 (hybrid-engine era), the modelling table has 4,626 rows and 13 model features. To avoid leakage and respecting the repository size limit, we excluded post-race tables (lap_times, pit_stops) and post-race standings . The dataset is chosen for two reasons; the target must be built from multiple tables and the time structure calls for careful split and feature design.

## Business / real-life framing

FantasyGP is a fantasy F1 platform, which will suggest drivers who are most likely to score points (top-10 finish) in the next race. The model runs after qualifying and before the race, so only the starting grid, the qualifying results and previous races can be used. This leads to three pipeline decisions:

- scores_points = (points > 0) * 1. This is equivalent to a top ten finish (zero mismatches) for 2014-2024, so the target is clear.
- Split: chronological, since a random split would allow predictions of past races to be influenced by future races. Train 2014–2019 (2,487 rows, 48.7% positive), validation 2020–2021 (780 rows, 50.0%), test 2022–2024 (1,359 rows, 50.0%).
- Threshold metric: expected misclassification cost. A false positive (recommending a non-scorer) has a cost of 1, a false negative (missing a scorer) costs 2, because the value of the platform is in finding undervalued drivers. This cost is minimised on the validation set to choose the threshold (which is not fixed at 0.5). With calibrated probabilities, the theoretical cut-off is 1/(1+2) ≈ 0.33.

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
