# Regression with an Abalone Dataset
[Kaggle competition: Playground Series - Season 4, Episode 4](https://www.kaggle.com/competitions/playground-series-s4e4)

**GOAL:** predict the age of abalone from various physical measurements.  
The evaluation metric for this competition is `Root Mean Squared Logarithmic Error`.  
**PRIVATE SCORE:** 0.14605 (Top 20% of leaderboard)

---
## Architecture
---

#### EDA

The features:
1. Sex (categoacal): M, F, and I (infant)
2. Length (continuous) - Longest shell measurement, mm
3. Diameter	(continuous) - perpendicular to length, mm
4. Height (continuous) - with meat in shell, mm
5. Whole weight (continuous) - whole abalone, grams
6. Shucked weight (continuous) - weight of meat, grams
7. Viscera weight (continuous) - gut weight (after bleeding), grams
8. Shell weight (continuous) - after being dried, grams
9. Rings (integer): +1.5 gives the age in years

Generated data was used for the competition. The training set was supplemented with [original data](https://www.kaggle.com/datasets/ravi20076/playgrounds4e04originaldata).

#### Choosing the default model

|                          | Train RMSLE | Test RMSLE |  Test R2  |
| ------------------------ | ----------- | ---------- | --------- |
|        Linear Regression |   0.049806  |  0.049200  | 0.667240  |
|         Ridge Regression |   0.049806  |  0.049200  | 0.667239  |
| Random Forest Regression |   0.017691  |  0.045806  |	0.716091  |
|       Bagging Regression |   0.020856  |  0.047807  |	0.690532  |
|       XGBoost Regression |   0.041198  |  0.045048  |	0.724207  |
|      LightGBM Regression |   0.044126  |  0.044750  |	0.727181  |
|      CatBoost Regression |   0.042644  |  0.044591  |	0.730209  |

CatBoost Regression with default parameters shows the best metrics on train data.

#### VotingRegressor with Optuna
Creating an ensemble of different multihead-models: `LightGBM x3`, `XGBoost Regression x3`, `CatBoost x3` are tuned with Optuna.  
Than VotingRegressor is tuned by Optuna too.
