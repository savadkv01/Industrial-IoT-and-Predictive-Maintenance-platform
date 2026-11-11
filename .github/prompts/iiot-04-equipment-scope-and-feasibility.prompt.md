---
description: "IIoT Stage 04: capture laptop and time budgets, select one equipment family and the approved data tracks, run a small feasibility probe, and set go/no-go gates for diagnosis versus forecasting."
agent: "agent"
---

# IIoT Stage 04: Equipment Scope and Feasibility

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- Outputs from earlier stages:
  - `docs/iiot/requirements.md`
  - `docs/iiot/domain/measurement-and-label-contracts.md`
  - `docs/iiot/sources/source-register.md`
  - `docs/iiot/sources/field-coverage-matrix.md`
  - `docs/iiot/sources/permitted-claims.md`
- If any of these files is missing, stop and name the stage that produces it.

## 1. Objective, scope and non-goals
- **Objective:** fix one coherent equipment family, the approved data tracks, an initial replay workload and resource envelopes, all backed by measured feasibility.
- **Non-goals:** full infrastructure and architecture selection (Stage 05). Installing services is out of scope unless the user approves a bounded probe.

## 2. Prerequisites and inputs
- Approved sources from Stage 03 and their profiles.
- The user's environment facts, which you collect in this stage.

## 3. Domain learning objective
Explain how the equipment family determines which failure modes and signals matter. Examples:
- **Hydraulic systems:** cooler degradation, valve condition, internal pump leakage and accumulator pressure.
- **Rotating machines:** imbalance, misalignment and bearing wear.

Also explain why diagnosing the current condition from a test rig is feasible, while forecasting time to failure needs longitudinal labels.

## 4. Decisions to put to the user
1. **Environment facts (free text):** CPU cores, RAM, free disk space, OS and WSL2 status, Docker Desktop or Engine, network limits, weekly time available. Never ask for credentials.
2. **Equipment family for the MVP:** (a) hydraulic system, backed by the measured UCI test-rig data (recommended when diagnosis is the MVP); (b) generic rotating equipment from a synthetic or AI4I-style source; (c) custom. Show the trade-offs: realism, available labels and learning value.
3. **Approved data tracks:** (a) measured diagnosis track plus a custom synthetic fleet for platform behaviour (recommended); (b) also add C-MAPSS as a separate RUL benchmark; (c) measured data only; (d) custom. Keep each track separate.
4. **Resource and retention envelopes:**
   - maximum RAM for the stack;
   - disk budget for raw, bronze, silver and gold data;
   - broker retention;
   - replay acceleration factor.

## 5. Bounded experiment (requires approval)
Run a small probe with plain Python and pyarrow, without Spark or Kafka unless approved. The probe should:
1. Replay N agreed cycles or assets at an agreed rate.
2. Write Parquet output.
3. Compute one simple feature.
4. Measure wall time, CPU time, peak RSS memory and bytes written.
5. Check label feasibility: class balance, number of independent groups and whether any future-horizon labels exist.

Report the measurements and extrapolate only with clearly labelled assumptions. Delete the probe output, or move it outside Git, as the user prefers.

## 6. Gates
1. Wait for the environment answers.
2. Get the probe plan approved.
3. Report the results.
4. Ask for the durable selections.

## 7. Work steps
1. Record the environment facts and budgets.
2. Document the equipment family, its components and the target signals.
3. Define the initial workload: assets, sampling, replay rate and duration.
4. Define go/no-go gates, for example:
   - the stack fits the RAM budget, with headroom;
   - a day of replay fits the disk budget;
   - diagnosis labels have at least K independent groups;
   - forecasting proceeds only if longitudinal failure labels with a defined horizon exist.
5. State the scope: diagnosis-only, diagnosis plus a synthetic forecasting demonstration, or a separate RUL benchmark.

## 8. Deliverables
- `docs/iiot/feasibility.md`: environment, budgets, probe method and results, and go/no-go gates.
- ADRs for the equipment family, the data tracks and the analytical scope.

## 9. Acceptance checks
- The probe results are reproducible with the documented command.
- Budgets are numeric.
- Every analytical capability is marked *feasible*, *demonstration only* or *out of scope*, with evidence.
- **Falsified if:** forecasting is declared feasible without longitudinal labels, or measured resource use exceeds the approved budgets.

## 10. Blockers and unverified items
If Docker or WSL2 is unavailable, record a blocker and offer a pure-Python fallback path. Mark extrapolations as estimates.

## 11. State, ADR and traceability updates
Update the project state, the ADR index and the requirement statuses, for example by deferring forecasting. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): record equipment scope and feasibility probe results`. Do not commit unless the user asks.
- Next prompt: `iiot-05-solution-and-data-architecture`.
