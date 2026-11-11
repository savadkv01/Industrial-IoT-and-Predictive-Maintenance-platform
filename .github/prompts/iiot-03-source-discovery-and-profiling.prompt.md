---
description: "IIoT Stage 03: verify candidate data sources (provenance, licences, availability, semantics), profile bounded samples, and build the source register, field-coverage matrix and permitted-claim matrix."
agent: "agent"
---

# IIoT Stage 03: Source Discovery and Profiling

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- The Stage 01 requirements and the Stage 02 domain contracts:
  - `docs/iiot/requirements.md`
  - `docs/iiot/domain/measurement-and-label-contracts.md`
- If either file is missing, stop and report which predecessor stage is incomplete.

## 1. Objective, scope and non-goals
- **Objective:** establish, from evidence, which sources may be used, for which claims, and under which terms.
- **In scope:** provenance, original documentation, licence and redistribution terms, download availability, units, sampling rates, cycle and asset identities, time semantics, labels, missing fields and profiling of bounded samples.
- **Non-goals:**
  - full-dataset ingestion;
  - choosing the final equipment family (Stage 04);
  - committing raw data to Git.

## 2. Candidate sources
Verify each candidate against its authoritative page. Recheck all of them; the brief may be out of date.

1. **UCI Condition Monitoring of Hydraulic Systems**: https://archive.ics.uci.edu/dataset/447/condition+monitoring+of+hydraulic+systems
   - Measured data from a test rig.
   - Fixed 60-second cycles with mixed sensor sampling rates.
   - Labels are component-condition grades.
   - Listed licence: CC BY 4.0.
2. **UCI AI4I 2020**: https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset
   - Synthetic data with current failure labels.
   - The failure-mode columns leak into the aggregate label.
   - Listed licence: CC BY 4.0.
3. **NASA C-MAPSS** via the PCoE repository: https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/
   - Simulated run-to-failure trajectories, measured in cycles.
   - Verify the dataset-specific terms.
4. **Microsoft Azure predictive-maintenance samples**
   - Establish the original provenance and permissions.
   - Kaggle mirrors with an Unknown licence are not approved.
   - The archived PySpark tutorial and the 100-machine five-file sample are not necessarily the same dataset.
   - A repository's code licence does not cover its data.
5. **Custom seeded simulator**: its own provenance track. It is designed in Stage 09; here, record only the fields it would need to supply.

## 3. Domain learning objective
Explain the following:
- A test rig with reset cycles differs from fleet operating history.
- Cycle order is not elapsed machine time.
- Each provenance track can support different claims.

## 4. Decisions to put to the user
1. **Which sources to sample now:** (a) UCI Hydraulic + AI4I (recommended; small and CC BY); (b) add C-MAPSS for an RUL benchmark; (c) add the Azure sample, but only if its provenance and permissions can be verified; (d) custom.
2. **Sample bounds:** propose the maximum download size, the disk location (outside Git, for example `data/raw/`, which goes into `.gitignore`) and the expected runtime. Ask for approval.
3. **Profiling tool:** (a) a small reusable profiling script using pandas or pyarrow, in a temporary location agreed with the user (recommended); (b) a throwaway notebook; (c) custom. Durable package paths are decided in Stage 08.
4. **Licence interpretation:** if the terms are ambiguous, ask whether to exclude the source, contact the publisher, or use it locally only, never redistributed.

## 5. Bounded experiment (requires approval)
For each approved source:
- Download only the agreed sample.
- Record the checksum, file sizes and row and cycle counts.
- Check sampling rates against the documentation.
- Record value ranges and units, the share of missing values, label distributions and time-field semantics.
- Report the measured numbers. Do not estimate.

## 6. Gates
Wait for approval of the sources, bounds and tool before any network or disk activity. Get approval of the plan before writing the register.

## 7. Work steps
1. Write a source register entry for each candidate source:
   - URL, publisher and access date;
   - licence and redistribution decision;
   - citation text;
   - provenance track;
   - status: approved, excluded or pending.
2. Build a field-coverage matrix: the desired fields (temperature, vibration, pressure, RPM, operating hours, status, maintenance, failures, asset metadata) against each source, marked *native*, *derivable with assumption* or *absent*.
3. Build a permitted-claim matrix: which analytical claims each source can support, such as diagnosis, anomaly detection, forecasting or RUL, and which it must not.
4. Write the profiling reports, with measurements only.

## 8. Deliverables
- `docs/iiot/sources/source-register.md`
- `docs/iiot/sources/field-coverage-matrix.md`
- `docs/iiot/sources/permitted-claims.md`
- `docs/iiot/sources/profiles/<source>.md`
- `.gitignore` entries for raw data, if the user approves them.

## 9. Acceptance checks
- Every approved source has a verified URL, a licence and an access date.
- Each profile's counts are reproducible by re-running the profiling command.
- The coverage matrix does not show operating hours, RPM or maintenance history for UCI Hydraulic unless the source documentation states them.
- **Falsified if:** any claim relies on row order as time, or treats public access as permission to redistribute.

## 10. Blockers and unverified items
If browsing or downloading is unavailable, say so and ask the user for the smallest piece of evidence needed, such as a pasted licence text or file listing. Mark the affected fields `unverified`.

## 11. State, ADR and traceability updates
- Record ADRs for source approval and exclusion.
- Update the project state with the measured evidence.
- Link the sources to their requirements.
- Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): register and profile candidate data sources`. Do not commit unless the user asks.
- Next prompt: `iiot-04-equipment-scope-and-feasibility`.
