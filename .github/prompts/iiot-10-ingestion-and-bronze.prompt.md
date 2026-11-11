---
description: "IIoT Stage 10: implement the approved transport and bronze persistence with envelopes, checksums and manifests, secure connections, backpressure, bounded retries, quarantine and a proven no-silent-loss recovery test."
agent: "agent"
---

# IIoT Stage 10: Ingestion and Bronze

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md), especially the transport, storage and ID ADRs
- `docs/iiot/architecture/architecture.md`
- `docs/iiot/data-model/event-envelope.md`
- `docs/iiot/governance/control-requirements.md`
- The Stage 09 replay and simulator modules

If they are missing, stop.

## 1. Objective, scope and non-goals
- **Objective:** move events from producers through the approved transport into bronze. Bronze must be append-only, traceable and recoverable.
- **Requirements:**
  - Keep the original source files, with their manifests and checksums, separate from the replay envelopes.
  - Quarantine malformed events.
- **Non-goals:**
  - normalization and deduplication semantics (Stage 11);
  - end-to-end exactly-once guarantees.

## 2. Prerequisites and inputs
- The broker and storage running under the Compose `core` profile.
- The security controls owned by this stage: broker auth and ACLs, TLS if approved, and secrets from the environment.

## 3. Domain learning objective
Explain store-and-forward at the industrial edge, and why historians buffer data during network loss. Relate this to producer retries, `acks=all` and idempotent producers. If MQTT was chosen, also cover QoS 1 duplicates.

## 4. Decisions to put to the user
1. **Topic and partition design:**
   - (a) one topic per provenance track and signal class, partitioned by asset ID (recommended for per-asset ordering);
   - (b) a single topic;
   - (c) custom.
   Also agree the partition count and retention, within the budget.
2. **Bronze write mode:**
   - (a) a micro-batch consumer that writes Delta/Parquet and commits offsets only after a durable write (at-least-once, recommended);
   - (b) a Spark Structured Streaming sink with a checkpoint;
   - (c) custom.
   Explain the duplicate implications.
3. **Quarantine policy:** what counts as malformed (schema, unit, required fields), where it is stored, and how a quarantined event is replayed.
4. **MQTT bridge (only if MQTT was selected):** (a) a small explicit bridge service with manual acknowledgement, QoS 1 and idempotent keys (recommended); (b) Kafka Connect MQTT source (check the licence); (c) custom. Document the replay limitation: MQTT does not retain history.

## 5. Research or experiment
- Confirm the client library versions and their idempotent-producer support.
- Propose a recovery test design and get approval before running long jobs.

## 6. Gates
Wait for the decisions. Get approval of the implementation plan, then implement.

## 7. Work steps
1. Raw file landing: copy the approved source files into the raw zone with a manifest (path, size, SHA-256, source ID, licence, retrieval time). Never mutate them.
2. Producer adapter: wrap the Stage 09 producers with:
   - authenticated connections;
   - idempotence or deterministic keys;
   - bounded retries with backoff;
   - backpressure.
3. Bronze consumer: validate the envelope, write valid events with the ingestion time and a batch ID, route invalid ones to quarantine with a reason code, and commit offsets only after a durable write.
4. Immediately run a focused test: write a small batch, then read it back.
5. Recovery test:
   - Kill the consumer and the broker mid-stream.
   - Restart them.
   - Reconcile the source counts against bronze, counting by event ID with duplicates allowed and gaps not allowed.
6. If MQTT was selected, implement and test the bridge, including a broker restart.
7. Document the delivery semantics for each hop.

## 8. Deliverables
- Ingestion modules, the CLI, Compose services or config, and tests.
- `docs/iiot/pipelines/bronze.md`, covering topics, layout, manifests, quarantine, delivery semantics and recovery results.
- ADRs.

## 9. Acceptance checks
- The recovery test shows zero missing event IDs. Duplicates are measured and reported.
- Malformed fixtures land in quarantine with reason codes.
- An unauthenticated or unauthorised publish is rejected, where auth is in scope.
- Raw manifests verify their checksums.
- **Falsified if:** the reconciliation finds gaps, offsets are committed before writes, or credentials are hard-coded.

## 10. Blockers and unverified items
State the remaining loss windows honestly, for example `acks=1`, or MQTT QoS 0 if it was used.

## 11. State, ADR and traceability updates
Update the project state with the recovery evidence, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(ingestion): persist telemetry to bronze with replay-safe event keys and quarantine`. Do not commit unless the user asks.
- Next prompt: `iiot-11-event-time-and-silver`.
