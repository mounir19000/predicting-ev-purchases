# XGBoost ablation study

These notebooks compare feature/encoding choices using frozen parameters, matched folds, and inner cross-fitted target encoding. They do not tune hyperparameters or generate test submissions.

| Notebook | Configuration | Completed historical run |
| --- | --- | --- |
| [Screening](xgboost_ablation_study.ipynb) | Three folds, six experiments: 18 fits | `outputs/screen_c141b1091456465b/` |
| [Confirmation](xgboost_ablation_confirmation.ipynb) | Ten folds, two split seeds, two configurations: 40 fits | `outputs/confirm_d0ef7dd4db72cd69/` |

Both runs are complete. The confirmed candidate removes 12 engineered ratio/charging helpers while retaining the raw numeric columns and other features.

## Results

The [screening summary](outputs/screen_c141b1091456465b/ablation_summary.csv) showed a mean OOF improvement of approximately 0.000029 for `no_ratios_charging` against `full_reference`.

The [confirmation summary](outputs/confirm_d0ef7dd4db72cd69/ablation_summary.csv) showed positive repeat-level deltas on both split seeds, averaging **+0.00000582**. The candidate improved 11 of 20 individual folds. Averaging predictions across both repeats yielded **0.9458045955**, compared with **0.9457967150** for the reference. These are small local gains; they do not establish a guaranteed leaderboard improvement.

The reference is the single recipe-residual XGBoost model. It is distinct from the advanced notebook's three-member blend. Compare candidates against the reference on the same folds rather than comparing screening and confirmation headline scores directly.

## Running experiments

Select the repository environment, save the notebook, restart the kernel, and run all cells. CPU is the default. Each fit allows up to 6,000 rounds with 200-round early stopping and uses five inner folds for supervised encodings.

Optional catalog entries cover recipe information, residual encodings, interactions, boundaries/logs, smoothing strength, and recipe-prior strength. These default variants start from the full reference; challenger-based experiments must also retain its ratio/charging exclusions.

## Artifacts and resuming

Runs write into `outputs/<mode>_<fingerprint>/`. Small tracked reports include:

- `ablation_summary.csv` / `.json`: results and comparisons.
- `repeat_oof_metrics.csv`, `paired_fold_deltas.csv`, `fold_metrics.csv`: repeated and fold-level evidence.
- `run_manifest.json`, `experiment_features.json`: settings and feature provenance.

OOF predictions and checkpoints remain local and ignored. Completed fits resume only when data, saved code, versions, and settings match. Save the notebook before running and avoid concurrent writers to the same run directory.

The cleanup changed notebook paths, so rerunning the reorganized notebooks creates new fingerprints. Historical runs remain preserved; see [migration notes](../README.md#completed-runs-and-migration).
