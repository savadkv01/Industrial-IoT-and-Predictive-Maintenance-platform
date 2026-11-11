---
description: "IIoT Stage 09: build reusable source-replay adapters and a seeded synthetic fleet simulator as separate provenance tracks, with documented assumptions and deterministic tests."
agent: "agent"
---

# IIoT Stage 09: Replay and Synthetic Fleet

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/data-model/event-envelope.md`
- `docs/iiot/data-model/source-to-target-mappings.md`
- `docs/iiot/domain/measurement-and-label-contracts.md`
- `docs/iiot/feasibility.md`
- `docs/iiot/dev-guide.md`

Stop if the Stage 08 skeleton is missing.

## 1. Objective, scope and non-goals
- **Objective:** produce event streams that can be replayed and reproduced, in two kinds:
  - (1) **source replay** of approved measured or benchmark data, keeping the original identity intact;
  - (2) a **seeded simulator** of a synthetic fleet, with documented physics-inspired assumptions.
- **Non-goals:**
  - publishing to the broker (the adapter interface only, unless the transport already exists);
  - bronze persistence (Stage 10);
  - claiming that the simulator is a validated digital twin.

## 2. Prerequisites and inputs
- Locally available approved sources (Stage 03 sample location).
- The envelope contract.
- The replay workload from Stage 04.

## 3. Domain learning objective
Explain the following:
- How operating regimes, such as load or ambient temperature, shift the sensor baselines. This is why regime context is needed to avoid false anomalies.
- How degradation shows up gradually, for example rising cooler outlet temperature or pressure ripple.

## 4. Decisions to put to the user
1. **Replay timing:**
   - (a) preserve the cycle-relative offsets and assign the replay time as `start + offset / acceleration`, labelled `replay_assigned_time` (recommended);
   - (b) emit as fast as possible;
   - (c) custom.
   Also confirm the acceleration factor.
2. **Mixed sampling rates (for example 100 Hz, 10 Hz and 1 Hz in UCI Hydraulic):** (a) emit each channel at its native rate (recommended for fidelity); (b) emit a pre-aggregated low-rate summary as well, as a separate stream; (c) custom.
3. **Simulator scope:** pick the elements to include: operating regimes, correlated signals, sensor drift, dropouts, stuck sensors, gradual degradation to failure, scheduled and reactive maintenance, and transport faults (duplicates, out-of-order or late events). Recommend a minimal set first.
4. **Fleet size and rates:** agree the number of assets and the events per second within the Stage 04 budget.

## 5. Research or experiment
- Briefly ground the simulator's assumptions in public references for the chosen equipment family, and cite them.
- Measure the generation throughput on a small run.

## 6. Gates
Wait for the decisions. Get approval of the design, covering the interfaces, the scenario configuration format and the file list, before coding.

## 7. Work steps
1. Define a `Source` or `EventProducer` interface in the approved package that yields envelope events, plus a sink interface (in-memory or file for now).
2. Implement the replay adapter for each approved source. It must:
   - carry the original file, cycle and sample identity and the `provenance_track`;
   - preserve the relative timing;
   - never duplicate cycles into fake assets.
3. Write a focused test immediately, checking ID stability and offset preservation on a tiny fixture.
4. Implement the seeded simulator with these elements:
   - a scenario config file;
   - a regime model;
   - correlated signals;
   - degradation;
   - sensor faults;
   - maintenance events;
   - a separate ground-truth label stream that is kept out of scoring inputs.
5. Add transport-fault injection (duplicates, reordering, delay) as an opt-in decorator.
6. Write the simulator assumptions document.

## 8. Deliverables
- Replay and simulator modules, a CLI command, tiny lawful fixtures and tests.
- `docs/iiot/simulation/simulator-assumptions.md`
- ADRs for replay timing and simulator scope.

## 9. Acceptance checks
- The same seed and config produce byte-identical output.
- Different seeds produce different output.
- Replay preserves the source identity and offsets.
- No ground-truth field appears in the telemetry envelope.
- The rate limiter stays within ±X% of the target.
- The measured and synthetic asset ID namespaces are disjoint.
- **Falsified if:** a test can recover the hidden labels from the telemetry stream fields, or replayed cycles are counted as independent assets.

## 10. Blockers and unverified items
Mark the simulator's realism as an *assumption*; it is unvalidated.

## 11. State, ADR and traceability updates
Update the project state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(replay): add provenance-preserving source replay and seeded fleet simulator`. Do not commit unless the user asks.
- Next prompt: `iiot-10-ingestion-and-bronze`.
