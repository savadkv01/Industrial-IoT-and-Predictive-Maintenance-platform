---
description: "IIoT Stage 13: build threshold, statistical and simple ML baselines, with separate, pre-registered evaluation protocols for diagnosis, anomaly detection, forecasting and optional RUL."
agent: "agent"
---

# IIoT Stage 13: Baselines and Model Evaluation

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md), especially the analytical scope ADRs
- `docs/iiot/sources/permitted-claims.md`
- `docs/iiot/features/feature-contract.md`
- `docs/iiot/domain/measurement-and-label-contracts.md`

## 1. Objective, scope and non-goals
- **Objective:** establish honest baselines. Fix each evaluation protocol before looking at its results.
- **Diagnosis and anomaly detection:** in scope whenever their prerequisites are met.
- **Forecasting and RUL:** in scope only if they were approved as feasible or as a separate benchmark. Otherwise, record them as deferred.
- **Non-goals:**
  - deep learning by default;
  - hyperparameter sweeps beyond the agreed budget;
  - combining results across provenance tracks.

## 2. Prerequisites and inputs
- Gold features and labels with their availability times.
- The scope decisions from Stage 04.

## 3. Domain learning objective
- Explain why random row splits inflate test-rig scores when cycles from the same experimental condition share characteristics.
- Explain why lead time and false alerts per machine-day matter more to maintenance planners than AUC.

## 4. Decisions to put to the user
1. **Baselines per capability.** Recommend one set:
   - **Diagnosis:** a per-component threshold or rule baseline, then a logistic regression or random forest on the cycle features.
   - **Anomaly detection:** a robust z-score or EWMA control chart, then an Isolation Forest.
   - **Forecasting:** a hazard or horizon classifier.
   - **RUL:** piecewise-linear regression on C-MAPSS.
2. **Splits:**
   - **Diagnosis:** grouped splits by experimental condition or run, where that grouping is known.
   - **Forecasting:** time- and asset-aware splits, with an embargo that covers the feature and label windows.
   - **RUL:** the official train/test splits.
   Ask the user to confirm each one.
3. **Metrics per capability:**
   - **Diagnosis:** macro-F1 and a confusion matrix per component.
   - **Anomaly detection:** event-level recall, false alerts per machine-day and detection delay.
   - **Forecasting:** recall at horizon H, lead-time distribution and precision at a fixed alert budget.
   - **RUL:** RMSE and the NASA scoring function, in cycles.
   For every metric, include uncertainty: bootstrap or cross-validation intervals.
4. **Horizon, censoring and rare events (forecasting only):** the horizon H, how to handle censored units, and class weighting versus threshold tuning.
5. **Experiment tracking now:** (a) a lightweight JSON or CSV run log with an artifact hash (recommended until Stage 19); (b) MLflow, if it was approved in Stage 05.

## 5. Research or experiment
- Pre-register the protocol in the documentation first.
- Agree a compute budget, including the maximum fits and runtime.

## 6. Gates
1. Wait for the decisions.
2. Get approval of the written protocol before running any evaluation.
3. Get approval of the implementation plan.

## 7. Work steps
1. Write the evaluation protocol per capability and track, and get it approved.
2. Implement training pipelines that fit all preprocessing on the training folds only. Use explicit feature allow-lists. In AI4I, exclude the failure-mode columns when predicting the aggregate label.
3. Run the baselines and record the metrics with their intervals, the data and feature versions, the seeds and the code version.
4. Write a model card per approved model, stating its intended use, limits and provenance track.

## 8. Deliverables
- Training and evaluation modules, and tests.
- `docs/iiot/models/evaluation-protocol.md`
- `docs/iiot/models/results.md`
- `docs/iiot/models/model-cards/`
- ADRs for the metrics, splits and selected models.

## 9. Acceptance checks
- A leakage test proves no label-derived column is in the feature allow-list.
- A split test proves there is no group or time overlap and the embargo is respected.
- Results can be reproduced from a seed.
- A simple baseline is reported next to every complex model.
- **Falsified if:**
  - results are pooled across tracks;
  - test data influences preprocessing or threshold selection;
  - forecasting metrics are reported without a horizon or for an unsupported source.

## 10. Blockers and unverified items
- Report weak results truthfully.
- State that synthetic or benchmark metrics do not transfer to real fleets.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(models): add baselines with pre-registered grouped and temporal evaluation`. Do not commit unless the user asks.
- Next prompt: `iiot-14-scoring-and-alert-lifecycle`.
