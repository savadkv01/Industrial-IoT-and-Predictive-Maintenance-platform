---
description: "IIoT Stage 06: model the industrial entities, telemetry, maintenance, labels, features, scores and alerts with explicit grains, IDs, units, multi-time semantics, versioning, provenance and source-to-target mappings."
agent: "agent"
---

# IIoT Stage 06: Data Modelling and Contracts

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/domain/measurement-and-label-contracts.md`
- `docs/iiot/sources/field-coverage-matrix.md`
- `docs/iiot/architecture/architecture.md`

If the architecture is not accepted, stop.

## 1. Objective, scope and non-goals
- **Objective:** write logical and physical data contracts that every later stage can implement and test.
- **Entities:**
  - sites, lines, assets and components;
  - component installation and replacement history;
  - sensor definitions and calibration;
  - telemetry and operating states;
  - maintenance events and failures;
  - replay/simulation runs and features;
  - scores and alerts;
  - inspection results;
  - resource usage.
- **Non-goals:** writing the implementation code. Executable schema files are allowed only if the user approves their location now; otherwise they wait for Stage 08.

## 2. Prerequisites and inputs
- The selected storage format and the bronze, silver and gold zones from the Stage 05 ADRs.

## 3. Domain learning objective
Explain the following:
- A sensor reading belongs to the component installed *at that time*, so component swaps reset degradation context.
- An alert acknowledgement is not maintenance completion.

## 4. Decisions to put to the user
1. **Time model:** (a) record event (observation) time, ingestion time, processing time and availability time, plus `replay_assigned_time` and `cycle_relative_offset` where they apply (recommended); (b) a reduced set with a written rationale; (c) custom. Store all timestamps in UTC with explicit timezone metadata.
2. **History handling:** (a) bitemporal (valid time plus recorded time) for asset, component and calibration dimensions, append-only for facts (recommended); (b) SCD2 only; (c) custom.
3. **Identifiers:**
   - (a) deterministic natural-key hashes for event IDs, so replay is idempotent (recommended);
   - (b) producer UUIDs plus sequence numbers;
   - (c) custom.
4. **Schema format and evolution:** (a) JSON Schema for envelopes plus Delta/Parquet table schemas (recommended without a registry); (b) Avro with a schema registry; (c) custom. Choose a compatibility rule, such as backward-compatible additions only.
5. **Telemetry grain:** keep the native per-channel sampling (long/narrow table), or resample to a wide table at a fixed rate. Explain the effect on storage, joins and fidelity.

## 5. Research or experiment
If the right telemetry grain is uncertain, propose a bounded storage comparison on the Stage 03 sample: write long and wide Parquet layouts and measure their size. Get approval first.

## 6. Gates
Wait for the decisions. Get approval of the model outline before writing.

## 7. Work steps
1. Draw an ER or logical diagram in Mermaid.
2. For each entity, record:
   - grain and keys;
   - columns with types, units and nullability;
   - time fields;
   - `provenance_track` (`measured` | `benchmark_simulated` | `synthetic`);
   - `source_id` and `schema_version`.
3. Define the event envelope: event ID, sequence number, source, asset, channel, value and unit, all time fields, quality flags, schema version and checksum.
4. Define label availability: when each label becomes known, plus censoring fields.
5. Write source-to-target mappings for each approved source, flagging every derived or assumed field.
6. Define the correction handling: late corrections, recalibration and restatement.

## 8. Deliverables
- `docs/iiot/data-model/data-model.md`
- `docs/iiot/data-model/source-to-target-mappings.md`
- `docs/iiot/data-model/event-envelope.md`
- Optionally, schema files at an approved path.
- ADRs for the time model, the IDs and the schema format.

## 9. Review criteria
- Every fact table states its grain and has an idempotent key.
- Every time field is defined.
- `provenance_track` is mandatory everywhere.
- **Falsified if:**
  - any mapping manufactures operating hours, RPM or maintenance history for sources that lack them;
  - `alert.acknowledged` implies maintenance completion;
  - the measured and synthetic tracks share asset IDs.

## 10. Blockers and unverified items
List any fields that remain `assumed` and the stage that must validate them.

## 11. State, ADR and traceability updates
Update the project state, the ADRs and the traceability table. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): define data model, event envelope and source mappings`. Do not commit unless the user asks.
- Next prompt: `iiot-07-governance-and-security-design`.
