# XGBoost experiments

Use [challenger/xgboost_no_ratios_charging_submission.ipynb](challenger/xgboost_no_ratios_charging_submission.ipynb) for the current best submission. Keep the other stages as references and controlled experiment tools.

| Directory | Purpose | Saved local OOF AUC |
| --- | --- | ---: |
| `baseline/` | Original raw-feature model | 0.94207978 |
| `advanced/` | Engineered features, target encoding, recipe prior, tuning, three-member rank blend | 0.94573396 |
| `ablation/` | Paired experiments establishing which feature groups help | See screening and confirmation summaries |
| `challenger/` | Confirmed feature selection, ten folds, two split seeds | **0.94580460** |

These stages use different validation setups; their headline scores are not paired comparisons. The challenger public leaderboard score is **0.94583**, as reported by the project owner on 2026-09-19.

## Working on the next improvement

1. Start from the current challenger configuration and establish its reference on the same splits as each candidate.
2. Use `ablation/` for controlled feature/encoding experiments. The existing catalog also supports target-encoding smoothing and recipe-prior-strength experiments; the default catalog reference retains the full feature set, so configure challenger-based comparisons explicitly.
3. Use `advanced/` when changing the tuning procedure. Its saved parameters came from the earlier feature configuration.
4. Confirm promising candidates across repeated splits before preparing a new submission.

Prediction and model files are local artifacts. Small run manifests, fold metrics, and summaries inside `outputs/<fingerprint>/` are tracked. Give a substantially different workflow a descriptive notebook name inside the relevant stage; avoid additional top-level `XGBoost_*` directories.

## Completed runs and migration

The repository was consolidated on 2026-09-19:

| Previous location | Current location |
| --- | --- |
| Original files directly inside `XGBoost/` | `XGBoost/baseline/` |
| `XGBoost_Advanced/` | `XGBoost/advanced/` |
| `XGBoost_Ablation/` | `XGBoost/ablation/` |
| `XGBoost_Challenger/` | `XGBoost/challenger/` |

All completed predictions, submissions, checkpoints, and model files were moved without changing their contents. Frozen configuration snapshots and historical run manifests retain their original paths and hashes as provenance; translate old path prefixes using the table above. They are historical records, not current filesystem links.

Updating notebook paths changes the saved-code hash. The reorganized ablation/challenger notebooks therefore create **new run fingerprints** rather than reusing pre-cleanup checkpoints. Do not rename old run directories or replace their hashes to force a resume. New runs can resume normally once their code and settings match.

Local copies of the original executed notebooks are archived as `source.ipynb` in the three completed screening, confirmation, and challenger run directories. They document the old code and outputs, use the old paths, and are ignored by Git. The current best submission is already available locally and does not need retraining after this move.
