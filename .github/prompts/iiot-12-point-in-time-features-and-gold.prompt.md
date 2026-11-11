---
description: "IIoT Stage 12: implement reusable trailing-window features and point-in-time maintenance and asset joins in gold, with offline and replay parity and targeted leakage tests."
agent: "agent"
---

# IIoT Stage 12: Point-in-Time Features and Gold

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- Stage 06 data model and label availability, `docs/iiot/data-model/data-model.md`
- Stage 11 silver documentation, `docs/iiot/pipelines/silver.md`
- Analytical scope ADRs from Stage 04

## 1. Objective, scope and non-goals
- **Objective:** a feature library where every value uses only records that were available at scoring time, and the streaming or replay path matches the offline recomputation.
- **Non-goals:** model training (Stage 13), a feature-store service, centred windows, whole-dataset scaling.

## 2. Prerequisites and inputs
- Silver telemetry with flags and context.
- Maintenance and component-installation dimensions.
- The label availability times.

## 3. Domain learning objective
Explain the following:
- **Condition indicators:** RMS, trend slope, pressure ripple, temperature delta to ambient or regime baseline.
- **Component replacement:** why a replacement must reset degradation features.
- **Retrospective leakage:** a maintenance record entered after the fact leaks future knowledge into the features.

## 4. Decisions to put to the user
1. **Feature set for the MVP:** offer a short list per signal family, sized to the approved equipment, and recommend a minimal, explainable set.
2. **Window definition:** (a) trailing event-time windows closed at the watermark, with the scoring time equal to the window end plus the availability lag (recommended); (b) per-cycle features for the test-rig diagnosis track; (c) custom. Keep the two provenance tracks' feature contracts distinct.
3. **Missingness and startup or idle handling:** minimum-coverage thresholds, excluding idle regimes, and whether a feature is emitted with a coverage ratio or suppressed.
4. **Maintenance joins:** use the record availability time, not the backdated event time. Ask how to treat maintenance records with unknown entry time.

## 5. Research or experiment
- Propose a parity experiment: compute features through the replay path and through the offline batch path on the same fixture, then diff them.
- Get approval for its size.

## 6. Gates
Wait for the decisions. Approve the feature contract and plan before coding.

## 7. Work steps
1. Write the feature contract. For each feature, record:
   - name, unit and window;
   - inputs and the regime/coverage requirements;
   - the reset rule;
   - provenance track;
   - version.
2. Implement pure, reusable feature functions shared by the batch and streaming paths.
3. Implement point-in-time as-of joins for asset, component, calibration and maintenance, using the availability time.
4. Build the gold feature tables with `feature_version`, `computed_at`, `as_of_time` and coverage.
5. Write the tests:
   - a leakage canary (inject a future-only marker and assert it never appears);
   - a component-reset test;
   - a startup/idle test;
   - a parity test between the offline and replay paths;
   - a no-centred-window assertion.

## 8. Deliverables
- Feature modules, gold jobs and tests.
- `docs/iiot/features/feature-contract.md`
- ADRs.

## 9. Acceptance checks
- The leakage canary passes.
- Offline and replay features match within the stated tolerance.
- Replacing a component resets the cumulative features.
- Each gold row's `as_of_time` is at or after the availability time of every input.
- **Falsified if:** any feature uses records stamped after its `as_of_time`, or scaling statistics are computed over the full dataset.

## 10. Blockers and unverified items
List any features whose physical meaning is assumed rather than verified.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(features): add point-in-time trailing features with leakage and parity tests`. Do not commit unless the user asks.
- Next prompt: `iiot-13-baselines-and-model-evaluation`.
