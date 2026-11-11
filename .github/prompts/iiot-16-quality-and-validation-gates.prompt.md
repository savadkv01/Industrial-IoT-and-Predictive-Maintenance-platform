---
description: "IIoT Stage 16: consolidate executable quality contracts for bronze, silver, gold and model inputs, define warn/fail/quarantine and freshness-aware promotion, and prove with injected faults that bad data cannot look healthy."
agent: "agent"
---

# IIoT Stage 16: Quality and Validation Gates

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/domain/measurement-and-label-contracts.md`
- `docs/iiot/data-model/data-model.md`
- The pipeline, feature and alert docs from Stages 10–14
- The quality checks already in the code from Stages 10–14

## 1. Objective, scope and non-goals
- **Objective:** turn the scattered quality checks into one coherent, executable contract and gate. Include a mechanism that blocks promotion when quality fails, and prove it with fault injection.
- **Non-goals:**
  - rewriting the working checks;
  - adding a heavyweight platform without justification;
  - postponing basic checks that earlier stages should already have.

## 2. Prerequisites and inputs
- An inventory of the existing checks; build it first, read-only.

## 3. Domain learning objective
Explain the following:
- **Sensor validation in industrial practice:** range checks, rate-of-change checks, redundancy and cross-sensor consistency checks (for example, pressure against flow).
- **Fail-safe interpretation of bad data:** the system must show an unknown status, never a healthy one.

## 4. Decisions to put to the user
1. **Framework:**
   - (a) keep the existing pytest/pandera-style checks inside the package (recommended if they are already present);
   - (b) Great Expectations or Soda Core;
   - (c) custom.
   Explain what each option costs in dependencies and learning.
2. **Severity policy per check:**
   - **warn:** the data is published, with a flag;
   - **fail:** promotion is blocked;
   - **quarantine:** the affected records are isolated.
   Propose a table and get confirmation.
3. **Freshness SLOs:** set the maximum acceptable lag for each zone and for scoring inputs, using event time and availability time rather than wall time.
4. **Source drift detection:** compare schema, distribution and sampling rate against reference profiles. Propose the thresholds.

## 5. Research or experiment
Inventory the existing checks and the gaps against the contract, then share the gap list before you implement anything.

## 6. Gates
Wait for the decisions, then get approval of the plan.

## 7. Work steps
1. Write a quality-contract registry. For each check, record its ID, layer, rule, severity, owner and test.
2. Cover these checks:
   - IDs and uniqueness;
   - units and ranges;
   - clock drift and future timestamps;
   - stuck sensors;
   - missing periods;
   - sampling-rate changes;
   - source drift;
   - availability of labels and features at scoring time;
   - provenance separation, with no mixed tracks in one model input.
3. Add a promotion gate between layers, plus a scoring gate. Failing data produces `insufficient_data` or `quality_blocked`, never `healthy`.
4. Write a fault-injection suite that feeds bad fixtures through the pipeline and asserts the expected gate outcome at each layer.
5. Publish quality results as data so Stage 17 can serve them and Stage 18 can monitor them.

## 8. Deliverables
- The consolidated check modules, the gate, the fault-injection tests and quality-results tables.
- `docs/iiot/quality/quality-contracts.md`
- ADRs.

## 9. Acceptance checks
- Every injected fault produces its expected gate outcome.
- No faulty fixture results in a `healthy` status or a valid prediction.
- The quality results carry run IDs.
- **Falsified if:** any bad fixture passes silently, or a stale feature is scored as current.

## 10. Blockers and unverified items
List the checks that need real-fleet references and cannot be validated locally.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(quality): consolidate quality contracts and fault-injection promotion gates`. Do not commit unless the user asks.
- Next prompt: `iiot-17-serving-and-maintenance-interface`.
