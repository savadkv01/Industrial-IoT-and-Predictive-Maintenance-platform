---
description: "IIoT Stage 02: teach and record the industrial domain model, covering asset hierarchy, measurement physics, maintenance and failure definitions, and the measurement and label contracts."
agent: "agent"
---

# IIoT Stage 02: Industrial Domain and Measurement

## Read first
Read these files in full, then follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md) and the accepted ADRs
- `docs/iiot/requirements.md` (Stage 01 output). If it does not exist yet, stop and ask the user to finish Stage 01.

## 1. Objective, scope and non-goals
- **Objective:** build a shared domain vocabulary and write the measurement and label contracts that later data modelling depends on.
- **In scope:**
  - asset → component → sensor hierarchy;
  - units and calibration;
  - operating regimes;
  - maintenance types (corrective, preventive, condition-based, predictive);
  - failure, fault and degradation definitions;
  - downtime and availability;
  - assumptions behind MTBF and MTTR;
  - sampling rate, Nyquist and signal bandwidth;
  - sensor faults versus equipment faults;
  - raw vibration waveforms versus low-rate indicators (RMS, kurtosis, peak).
- **Non-goals:** selecting data sources (Stage 03) and writing schemas or code (Stage 06 onward).

## 2. Prerequisites and inputs
- Approved requirements from Stage 01. If the equipment family is still undecided, write the contracts generically and add an annex for each candidate family: hydraulic system, compressor, pump, cooling.

## 3. Domain learning objective
Explain the teaching points below briefly and tie each one to a requirement:
- A 1 Hz "vibration" channel cannot reveal bearing defect frequencies in the kHz range.
- A stuck or drifting sensor is a data-quality event, not an equipment fault.
- MTBF and MTTR depend on how failure is defined and on the observation window.

## 4. Decisions to put to the user
1. **Failure definition used throughout the project:** (a) functional failure, meaning the component can no longer meet its required function (recommended); (b) any corrective maintenance event; (c) a source-defined label only; (d) custom. Explain how each choice affects labels and leakage.
2. **Condition-state vocabulary:** (a) the ordinal condition grades from the source, such as optimal, reduced efficiency or close to total failure (recommended when diagnosis is the MVP); (b) a binary healthy/faulty split; (c) custom.
3. **Reliability KPIs to compute, and only where the data supports them:** availability, MTBF, MTTR, alert precision, inspection lead time. Ask which ones to include, and flag any KPI that needs data the project may not have.
4. **Unit system and canonical units:** SI with explicit unit columns (recommended), or keep the source units and convert at read time.

## 5. Research or experiment
Cite authoritative public references for the definitions, for example ISO 13372 and ISO 14224 terminology summaries, or vendor-neutral reliability texts. Record the URL and access date. Paraphrase; do not copy standards text. No downloads are needed.

## 6. Gates
Wait for the answers. Then propose the outline of both documents and get approval before writing them.

## 7. Work steps
1. Write the glossary: each term, a plain-language meaning, why it matters here and a common misuse.
2. Write the measurement contract for each candidate signal family:
   - physical quantity and canonical unit;
   - typical range and plausible physical limits;
   - sampling-rate class;
   - calibration and offset metadata;
   - quality flags;
   - sensor-fault signatures (stuck, spike, dropout, drift, clipping).
3. Write the label contract:
   - label types: condition grade, fault class, failure event, RUL cycles;
   - when each label becomes known (availability time);
   - censoring;
   - what each label can and cannot evidence;
   - an explicit statement that simulated labels do not validate physical degradation.
4. Map the contracts to the requirement IDs.

## 8. Deliverables
- `docs/iiot/domain/glossary.md`
- `docs/iiot/domain/measurement-and-label-contracts.md`
- ADRs for the failure definition and the label vocabulary.

## 9. Review criteria
- Every signal has a unit, a sampling class and sensor-fault signatures.
- Every label states its availability time and the claims it can support.
- **Falsified if:** the contracts let a low-rate summary support high-frequency diagnostics, or let a maintenance record stand in for a failure without stated assumptions.

## 10. Blockers and unverified items
Mark source-specific facts, such as exact sampling rates, as `to verify in Stage 03`.

## 11. State, ADR and traceability updates
Update the project state and the ADR index. Add the new requirement links to the traceability table. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): add domain glossary and measurement/label contracts`. Do not commit unless the user asks.
- Next prompt: `iiot-03-source-discovery-and-profiling`.
