---
description: "IIoT Stage 19: implement model, feature and data versioning; reproducible training; artifact integrity; human-approved promotion and rollback; and drift monitoring that is kept separate from delayed-label performance."
agent: "agent"
---

# IIoT Stage 19: Model Lifecycle and Drift

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/models/evaluation-protocol.md`
- `docs/iiot/models/results.md`
- The model cards
- `docs/iiot/alerts/alert-lifecycle.md`
- `docs/iiot/operations/observability.md`

## 1. Objective, scope and non-goals
- **Objective:** make every scoring model traceable, reproducible, promotable only with approval, and reversible. Monitor drift, and evaluate performance only once the labels have matured.
- **Non-goals:**
  - automatic retraining or deployment;
  - claiming adaptation to the real world from synthetic drift tests.

## 2. Prerequisites and inputs
- The Stage 13 training pipelines and run logs.
- The Stage 14 scoring with `model_version`.

## 3. Domain learning objective
Explain the following:
- **Intervention bias:** a successful maintenance intervention prevents the failure it predicted, so the "missed" label is censored.
- **Seasonal and regime shifts:** these look like drift, but they are not model decay.

## 4. Decisions to put to the user
1. **Registry:**
   - (a) a file-based registry, using a versioned directory with a manifest, hashes and a model card (recommended unless MLflow is already approved);
   - (b) the MLflow tracking server and model registry, under the `ml` profile, justified by the learning goal;
   - (c) custom.
2. **Promotion rule:** what evidence the user needs, such as protocol metrics, no regression against the current champion, and a model card review. Promotion is always a manual approval step.
3. **Drift monitors:**
   - feature drift (PSI, KS test or a population summary) for each regime;
   - prediction-distribution drift;
   - alert-rate drift.
   Agree the thresholds and the actions. Drift must open a review, not trigger retraining.
4. **Delayed-label evaluation:** label maturity windows, and handling of censored outcomes when maintenance intervenes.

## 5. Research or experiment
- Confirm the MLflow version and its resource cost, if MLflow is considered.
- Design a synthetic drift scenario using the Stage 09 simulator, and get its approval.

## 6. Gates
Wait for the decisions, then get approval of the plan.

## 7. Work steps
1. Version the model artifacts with code SHA or package version, data and feature versions, training-config hash, metrics and the artifact SHA-256.
2. Make training reproducible from a single command and config. Run the check: retrain and compare the hashes or metrics within tolerance.
3. Implement the promotion and rollback commands, with an audit record, and make scoring pick up the promoted version.
4. Implement the drift monitors as gold tables and metrics, separate from performance tables. Run the performance evaluation only on matured labels.
5. Demonstrate the full flow:
   - a synthetic drift scenario triggers a review;
   - the challenger is evaluated;
   - the user approves or rejects promotion;
   - a rollback restores the previous version.

## 8. Deliverables
- Lifecycle modules and CLI commands, the drift jobs and tests.
- `docs/iiot/models/lifecycle.md`
- Updated model cards.
- ADRs.

## 9. Acceptance checks
- The artifact hash is verified before loading.
- After a rollback, the scores match the previous version on a fixture.
- Drift tables never mix provenance tracks.
- Performance is not computed on immature labels.
- **Falsified if:**
  - promotion can happen without an approval record;
  - drift automatically retrains or deploys a model;
  - synthetic drift results are described as real-world adaptation.

## 10. Blockers and unverified items
Record the drift thresholds as provisional.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(mlops): add versioned registry, approved promotion/rollback and drift monitors`. Do not commit unless the user asks.
- Next prompt: `iiot-20-security-and-governance-verification`.
