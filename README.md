# Predicting Electric Vehicle Purchases

Kaggle [Playground Series S6E9](https://www.kaggle.com/competitions/playground-series-s6e9): predict `Will_Buy_EV`, evaluated with ROC AUC.

The current best submission is the **XGBoost challenger**, with **0.94583 public leaderboard AUC** reported by the project owner and **0.9458045955 local OOF AUC**. Start with [the XGBoost guide](XGBoost/README.md).

## Repository layout

```text
EDA.ipynb                         Exploratory analysis
XGBoost/
  baseline/                      Original XGBoost model
  advanced/                      Feature engineering and Optuna search
  ablation/                      Controlled screening and confirmation
  challenger/                    Current best submission pipeline
CatBoost/                        Earlier CatBoost baseline
LightGBM/                        Earlier LightGBM baseline
RandomForest/                    Earlier Random Forest baseline
playground-series-s6e9/           Competition CSVs (local, ignored)
```

The four XGBoost stages share one parent directory. Keep future XGBoost experiments inside this structure; use run-specific output directories instead of adding more top-level model folders.

## Run locally

Use the existing `venv`, or create a Python 3.12 environment and install [requirements.txt](requirements.txt). These pins record the environment used for the saved runs.

```bash
python3.12 -m venv venv
venv/bin/python -m pip install -r requirements.txt
venv/bin/python -m jupyterlab
```

Download and extract `train.csv`, `test.csv`, and `sample_submission.csv` from the competition into `playground-series-s6e9/`. The training set has 668,665 rows; the test set has 286,571. Data access requires accepting the competition rules on Kaggle.

Open the selected notebook in that environment. Model notebooks resolve paths from the repository root or their own folder and write results into their model folder. Open `EDA.ipynb` from the repository root. Running a training notebook starts its configured experiments; the challenger trains 20 models if no matching checkpoints exist. Notebooks do not submit to Kaggle automatically.

## Current submission

The existing local file is:

```text
XGBoost/challenger/submission_xgboost_no_ratios_charging.csv
```

It contains `id,Will_Buy_EV`. Its recorded SHA-256 is:

```text
40491d5de4744315ec6d881eeae02916e31aa4a6766dadaf3a52d53973260a99
```

The completed run's [summary](XGBoost/challenger/outputs/157cd35ebfd96f73/run_summary.json), [manifest](XGBoost/challenger/outputs/157cd35ebfd96f73/run_manifest.json), and [leaderboard record](XGBoost/challenger/leaderboard_record.json) are versioned. See the [migration notes](XGBoost/README.md#completed-runs-and-migration) before resuming an older run.
