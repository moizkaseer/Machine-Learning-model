# FIFA World Cup 2026 Winner Prediction — Project Report

## What the Project Does

This project is a data mining pipeline that predicts the most likely winner of the 2026 FIFA World Cup. It collects historical international football results, engineers performance features for each team, stores the data in a SQLite data warehouse, and trains five classification models to learn which team characteristics correlate with winning a World Cup. The best model is then applied to the 48 teams qualified for 2026 to produce a ranked list of win probabilities.

## Data Used

- **Match results (1872–2024)** from the open `martj42/international_results` dataset — date, teams, scores, tournament, and neutral-ground flag.
- **Official FIFA rankings (2023–2024)** as CSV files — each team's rank and confederation by date.

## Data Warehouse

A SQLite database (`world_cup_dw.db`) built as a star schema:
- **Match_Facts** — fact table with scores, team IDs, tournament ID, date ID, neutral flag
- **Team_Dim**, **Tournament_Dim**, **Date_Dim** — dimension tables

## Feature Engineering

For each team in each World Cup (1998–2022), six features were computed using a two-year window before the tournament: win rate, goals per game, goals conceded per game, FIFA rank, host status, and confederation. The target is `is_winner` (1 = won the tournament, 0 = did not). Matches from the current tournament were excluded to prevent data leakage.

## Classification Models

Binary classification with a temporal split — trained on 1998–2018 and validated on 2022 (won by Argentina). Five models were compared: Logistic Regression, Decision Tree, Naive Bayes, Random Forest, and XGBoost. Class imbalance (one winner per ~32 teams) was handled with balanced class weights. Models were evaluated on accuracy, precision, recall, and F1. **Logistic Regression** generalized best and was chosen as the final model.

## 2026 Prediction

The final model was applied to all 48 qualified teams for 2026, using the same two-year feature window ending on the tournament start date. The output is a ranked win-probability list, with the three hosts (USA, Canada, Mexico) flagged for home advantage.

## Tools

Python (pandas, numpy, scikit-learn, XGBoost), SQLite for the data warehouse, and Google Colab for development.
