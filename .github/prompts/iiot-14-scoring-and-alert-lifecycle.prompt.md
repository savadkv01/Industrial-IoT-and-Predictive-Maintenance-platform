---
description: "IIoT Stage 14: build versioned scoring and an advisory inspection queue with evidence, thresholds, persistence, hysteresis, deduplication, acknowledgement states and audit history, with no physical control."
agent: "agent"
---

# IIoT Stage 14: Scoring and Alert Lifecycle

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/models/results.md` and the model cards
- `docs/iiot/features/feature-contract.md`
- `docs/iiot/data-model/data-model.md`, specifically the alert and inspection entities
- `docs/iiot/governance/control-requirements.md`

## 1. Objective, scope and non-goals
- **Objective:** score the gold features with approved model and rule versions, and turn the scores into an advisory inspection queue. The queue must be idempotent under replay.
- **Non-goals:** work-order automation, physical commands, and presenting anomaly scores as calibrated probabilities.

## 2. Prerequisites and inputs
- An approved baseline or model, with its thresholds.
- The serving store from Stage 05, for example PostgreSQL.

## 3. Domain learning objective
Explain the following:
- alarm flooding and alarm-management practice in the ISA-18.2 spirit;
- why persistence, deadband or hysteresis, and cooldowns reduce nuisance alerts;
- the steps between an alert and completed maintenance: triage, inspection, finding, maintenance.

## 4. Decisions to put to the user
1. **Alert rule:**
   - (a) score above the threshold for N consecutive windows, cleared below a lower threshold, with a cooldown (recommended);
   - (b) a single-window threshold;
   - (c) custom.
   Choose N, the thresholds and the cooldown from the Stage 13 evidence.
2. **Lifecycle states:** propose `open → acknowledged → inspection_scheduled → inspected (finding: confirmed/not-confirmed/inconclusive) → closed`. Maintenance completion is a separate event. Confirm the states or adjust them.
3. **Stale and insufficient data:** when features are stale or coverage is low, choose between (a) emitting an `insufficient_data` status rather than a healthy score (recommended) and (b) suppressing output entirely.
4. **Priority ranking:** severity × confidence × asset criticality, where criticality is user-provided metadata; or custom.

## 5. Research or experiment
Replay a fixture twice and count the alerts, as a design check on idempotency.

## 6. Gates
Wait for the decisions, then get approval of the plan.

## 7. Work steps
1. Write a scoring module that loads the approved model or rule version and checks feature-contract compatibility. Each score records:
   - `model_version`;
   - `feature_version`;
   - `as_of_time`;
   - evidence (the top contributing features and the raw values with units).
2. Write the alert engine:
   - deterministic alert keys made from asset, rule, episode start and model version;
   - persistence, hysteresis and cooldown;
   - deduplication through upserts.
3. Implement the queue tables and state transitions with an append-only audit history: actor, time, from and to states, and comment.
4. Implement stale-model and stale-feature handling.
5. Write the tests:
   - a replay produces no duplicate alerts;
   - hysteresis prevents flapping;
   - acknowledgement does not set maintenance completion;
   - stale input produces `insufficient_data`;
   - no code path writes to an external control interface.

## 8. Deliverables
- Scoring and alert modules, the database migrations and tests.
- `docs/iiot/alerts/alert-lifecycle.md`, with a state diagram.
- ADRs.

## 9. Acceptance checks
- Two identical replays produce identical alert sets.
- The flapping fixture yields one alert episode.
- The audit trail is complete for each transition.
- **Falsified if:**
  - duplicate alerts appear after a restart;
  - a score is presented as a failure probability without calibration evidence;
  - an acknowledgement implies that maintenance is done.

## 10. Blockers and unverified items
Mark the threshold values as provisional until Stage 19 monitoring.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(alerts): add idempotent scoring and advisory inspection queue with audit trail`. Do not commit unless the user asks.
- Next prompt: `iiot-15-airflow-and-recovery`.
