---
description: "IIoT Stage 05: compare and select transport, storage, compute, orchestration and serving options, confirm Python/Spark/Airflow roles, and produce ADRs and Mermaid architecture diagrams within the laptop envelope."
agent: "agent"
---

# IIoT Stage 05: Solution and Data Architecture

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md), including the accepted ADRs for equipment, data tracks and scope
- `docs/iiot/requirements.md`
- `docs/iiot/feasibility.md`
- `docs/iiot/sources/permitted-claims.md`

If the feasibility gates have not passed, stop.

## 1. Objective, scope and non-goals
- **Objective:** produce an architecture that meets the requirements within the measured laptop envelope, with the user approving every component choice.
- **Non-goals:** writing code or Compose files (Stage 08), STAC, Kubernetes and paid cloud services.

## 2. Prerequisites and inputs
- Accepted ADRs from Stages 01–04.
- Resource budgets from `docs/iiot/feasibility.md`.

## 3. Domain learning objective
Explain the following concepts:
- **MQTT:** a device-edge protocol with QoS 0, 1 and 2 levels and retained messages. It does not provide a durable, replayable log.
- **Kafka:** a durable, partitioned, replayable log.
- **Delivery semantics:** whether delivery is at-least-once or exactly-once depends on the sink. Duplicate alerts are therefore prevented through idempotent keys, not through the broker.
- **Purdue-model framing:** the platform sits at the business/analytics level and never touches control.

## 4. Decisions to put to the user (2–4 per interaction, across several rounds)
1. **Transport:**
   - (a) Kafka only, with replay producers (recommended: fewer moving parts and replayable);
   - (b) MQTT (Mosquitto) at the edge plus an explicit MQTT→Kafka bridge (more realistic, with a more complex delivery chain);
   - (c) no broker; file drops plus micro-batches (cheapest);
   - (d) custom.
2. **Kafka distribution, if Kafka is chosen:** (a) Apache Kafka in KRaft mode, single node; (b) Redpanda, which uses less memory but is a different product; (c) custom. Discuss licences and RAM.
3. **Stream and batch compute:**
   - (a) Spark Structured Streaming plus Spark batch (keeps the baseline, but is heavier);
   - (b) a lightweight Python streaming consumer with Spark for batch and backfill (recommended if RAM is tight);
   - (c) Polars or DuckDB only;
   - (d) custom.
   Confirm the roles of Python, Spark and Airflow explicitly.
4. **Medallion storage format:** (a) Delta Lake on the local filesystem (recommended for MERGE and time travel); (b) plain partitioned Parquet with manifests; (c) Apache Iceberg; (d) custom.
5. **Serving store:** (a) PostgreSQL for alerts, the queue and serving tables (recommended); (b) DuckDB files; (c) custom.
6. **Optional services and profiles:** MLflow, Prometheus and Grafana. Ask whether to include each one, and whether to place it behind a Compose profile so it can be switched off.
7. **Offline operation:** must the demo run fully offline after the initial pulls?

## 5. Research or experiment
- Check current stable versions and licences of the candidate components from their official sources, and record the access dates.
- If the RAM fit is uncertain, propose a bounded probe, such as starting a single-node broker and measuring its RSS, and get approval before running it.

## 6. Gates
Make decisions in rounds and wait after each one. Then get approval of the documentation plan before writing diagrams.

## 7. Work steps
1. Write the architecture document:
   - context;
   - quality attributes;
   - component responsibilities and boundaries, keeping domain logic in reusable packages;
   - data flow from raw → bronze → silver → gold → serving;
   - delivery semantics per hop;
   - failure paths;
   - a resource envelope for each service.
2. Write Mermaid diagrams: context, component, data flow, deployment (Compose services, volumes and profiles) and failure/recovery path.
3. Map each requirement to the components that satisfy it.

## 8. Deliverables
- `docs/iiot/architecture/architecture.md`, with the Mermaid diagrams embedded or in sibling `.md` files.
- One ADR for each component decision.

## 9. Review criteria
- Every component has a justified role and a resource budget, and the sum fits the RAM and disk envelopes.
- Delivery semantics are stated for each source → processing → sink hop.
- The diagrams render. Validate the Mermaid syntax if a validator is available.
- **Falsified if:** the design depends on paid cloud services, STAC, a GPU or Kubernetes, claims end-to-end exactly-once delivery, or places domain logic in DAGs or adapters.

## 10. Blockers and unverified items
Mark resource estimates that have not been measured as `estimate — verify in Stage 21`.

## 11. State, ADR and traceability updates
Update the ADRs, the project state and the traceability table. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): define solution architecture and component ADRs`. Do not commit unless the user asks.
- Next prompt: `iiot-06-data-modelling-and-contracts`.
