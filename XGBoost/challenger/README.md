# Current best XGBoost submission

The completed `no_ratios_charging` challenger achieved **0.9458045955 local OOF AUC** and **0.94583 public leaderboard AUC** reported by the project owner. The local run is `outputs/157cd35ebfd96f73/`; see its [summary](outputs/157cd35ebfd96f73/run_summary.json) and the [leaderboard record](leaderboard_record.json).

## File to submit

The existing local submission is:

```text
XGBoost/challenger/submission_xgboost_no_ratios_charging.csv
```

Its archive is `outputs/157cd35ebfd96f73/submission_xgboost_no_ratios_charging.csv`. Both files have SHA-256 `40491d5de4744315ec6d881eeae02916e31aa4a6766dadaf3a52d53973260a99` and contain 286,571 rows with only `id` and `Will_Buy_EV`. CSVs, predictions, checkpoints, and models are local and ignored; a fresh clone must recreate them or restore a separate backup.

## Training recipe

- 193 features, removing 12 engineered ratio/charging helpers while retaining raw numeric inputs.
- Ten stratified folds and two split seeds: 20 fits.
- Frozen parameters, class weight 1.0, recipe base margin, five-fold cross-fitted target encoding, `max_bin=512`, up to 6,000 rounds and 200-round early stopping.
- Equal averaging of test probabilities across folds and split seeds. This is one XGBoost recipe across repeated splits.

To train, open [xgboost_no_ratios_charging_submission.ipynb](xgboost_no_ratios_charging_submission.ipynb), select the repository environment, save, restart the kernel, and run all cells. The notebook validates row counts, IDs, finite probability bounds, and CSV round trips. It does not run Optuna or upload to Kaggle.

## Saved artifacts and resuming

Each run lives in `outputs/<fingerprint>/` and includes a manifest, metrics, features, prediction checkpoints, models, and an archived submission. The convenient submission above is updated only after all fits complete.

Matching completed fits resume with `RESUME_COMPLETED_FOLDS=True`. Changing code, versions, device, threads, or settings creates another fingerprint. The cleanup changed notebook paths, so the reorganized notebook will create a new run instead of reusing the historical checkpoints. The current submission remains ready to use without rerunning. See [migration notes](../README.md#completed-runs-and-migration).

Native model files do not contain the fold-specific target encoders or recipe margins; inference must recreate that preprocessing. Prediction checkpoints already contain the validation/test predictions required to reconstruct the completed submission. Avoid concurrent writers to the same run directory.
