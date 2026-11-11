---
description: "IIoT Stage 11: implement silver processing covering unit normalization, schema evolution, calibration and state context, quality flags, deduplication, event-time windows, watermarks, late-event policy and reconciliation."
agent: "agent"
---

# IIoT Stage 11: Event Time and Silver

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- Stage outputs:
  - `docs/iiot/data-model/data-model.md`
  - `docs/iiot/domain/measurement-and-label-contracts.md`
  - `docs/iiot/pipelines/bronze.md`
- The bronze implementation from Stage 10

## 1. Objective, scope and non-goals
- **Objective:** produce deterministic, deduplicated and unit-normalized silver telemetry with context and quality flags, under a defined late-data policy.
- **Non-goals:**
  - features and windows used for modelling (Stage 12);
  - forward-filling across gaps without explicit flags.

## 2. Prerequisites and inputs
- Bronze tables and quarantine.
- The compute engine ADR (Spark Structured Streaming or a lightweight consumer).

## 3. Domain learning objective
Explain the following:
- clock skew in PLCs and gateways;
- why plant historians use deadband and compression;
- how a stuck sensor (zero variance) differs from a genuinely stable process.

## 4. Decisions to put to the user
1. **Lateness bound and watermark:**
   - (a) a bounded streaming watermark, for example a few minutes of event time, plus a periodic batch reconciliation for data that arrives later than that (recommended);
   - (b) a large watermark only, which costs memory;
   - (c) batch only;
   - (d) custom.
   Explain the memory and state implications.
2. **Deduplication key and horizon:** by event ID within the watermark, plus a batch dedup in reconciliation (recommended); or custom.
3. **Gap handling:** (a) never impute in silver, only flag gaps (recommended); (b) limited interpolation with a flag; (c) custom.
4. **Clock problems:** the handling of future-dated, non-monotonic or skewed timestamps. Choose between quarantine, flag and correct-with-provenance.

## 5. Research or experiment
- If the watermark size is uncertain, propose measuring the out-of-order distribution in the Stage 09 fault-injected streams.
- Measure the state size against the watermark.

## 6. Gates
Wait for the decisions, then get approval of the plan.

## 7. Work steps
1. Normalize the units using the measurement contract, and record the original unit and value.
2. Join the calibration, operating state and component installation context as of the event time, never later.
3. Apply quality flags: range, rate of change, stuck, dropout, clipping and clock.
4. Deduplicate and apply event-time windowing with the agreed watermark. Route events that are too late for streaming to a reconciliation path.
5. Implement the reconciliation job, which recomputes the affected silver partitions deterministically.
6. Handle schema evolution according to the Stage 06 compatibility rule.
7. Run a focused test right after the first transform, then extend the tests to cover:
   - out-of-order events;
   - duplicates;
   - missing and stuck sensors;
   - restart from checkpoint;
   - schema version change;
   - the same input producing the same output in a different order.

## 8. Deliverables
- Silver modules, the reconciliation job and tests.
- `docs/iiot/pipelines/silver.md`
- ADRs for the lateness and gap policies.

## 9. Acceptance checks
- Processing the same bronze data in shuffled order produces identical silver output.
- Duplicates are removed.
- Late events beyond the bound are reconciled, not lost.
- No silver row uses context that was valid only after its event time.
- Stuck and dropout fixtures are flagged.
- **Falsified if:** forward-fill leaks values across gaps, or the restart test changes the output.

## 10. Blockers and unverified items
Record any limits on state size or memory that were observed.

## 11. State, ADR and traceability updates
Update the project state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(silver): normalize, deduplicate and window telemetry by event time`. Do not commit unless the user asks.
- Next prompt: `iiot-12-point-in-time-features-and-gold`.
