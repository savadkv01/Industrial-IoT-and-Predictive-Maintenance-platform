---
description: "IIoT Stage 07: agree on the threat model, exposure, authentication and authorization, broker permissions, data classification, licensing, retention, lineage, model approvals and secret lifecycle as executable control requirements."
agent: "agent"
---

# IIoT Stage 07: Governance and Security Design

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/architecture/architecture.md`
- `docs/iiot/data-model/data-model.md`
- `docs/iiot/sources/source-register.md`

If the architecture or the data model is missing, stop.

## 1. Objective, scope and non-goals
- **Objective:** turn governance and security intent into control requirements that later stages can test, and write a lawful policy for fixtures and publication.
- **Non-goals:**
  - implementing the controls (Stages 08–19 implement them; Stage 20 verifies them);
  - safety certification;
  - IEC 62443 or other OT compliance claims.

## 2. Prerequisites and inputs
- Accepted component ADRs and the licence decisions from the source register.

## 3. Domain learning objective
Explain the following concepts:
- **IT/OT segmentation:** why analytics platforms read from the operational side but never write back to it.
- **Broker abuse:** how unauthenticated MQTT and Kafka topics get exploited, through spoofed telemetry or topic flooding.
- **Advisory outputs:** why outputs must stay advisory.

## 4. Decisions to put to the user
1. **Network exposure:** (a) every service bound to `127.0.0.1` or an internal Compose network, with only the API and dashboard published to localhost (recommended); (b) LAN-accessible demo; (c) custom.
2. **Authentication and authorization:**
   - (a) broker SASL/SCRAM or MQTT username/password plus topic ACLs, and API token or basic auth with read and acknowledge roles (recommended baseline);
   - (b) local-only, with no auth, and this residual risk documented;
   - (c) mutual TLS;
   - (d) custom.
3. **Secret lifecycle:** (a) an ignored `.env` built from a committed `.env.example` with placeholders, plus generation and rotation instructions (recommended); (b) Docker secrets files; (c) custom. Never put real secrets in chat.
4. **Retention and deletion for each zone:** propose defaults for raw, bronze, silver and gold data, broker data, logs, models and backups, within the Stage 04 disk budget.
5. **Model approval:** who approves the promotion of a scoring model and its thresholds, for example the user as reliability-engineer persona, and what evidence they need.

## 5. Research or experiment
- Gather the security documentation for the selected components (Kafka or Mosquitto ACLs, Postgres roles, Grafana auth) from official sources.
- Re-confirm the licence attribution requirements from the source register.

## 6. Gates
Wait for the decisions, then get approval of the document plan.

## 7. Work steps
1. Build a threat model in STRIDE style over the data-flow diagram: assets, trust boundaries, threats, controls and residual risk.
2. Classify data per zone and provenance track (public-measured, public-simulated, synthetic, operational metadata, secrets).
3. Write the control requirements with IDs (`SEC-nn`, `GOV-nn`). For each one, give the owning stage and an executable verification, such as "unauthenticated publish to telemetry topic is denied".
4. Define the lineage and audit requirements: run IDs, input checksums, code version, model version and the audit trail for alert acknowledgements.
5. Write the fixture and publication policy:
   - which data may be committed, at what size and with what attribution;
   - screenshot sanitisation;
   - a list of items that must never be committed.

## 8. Deliverables
- `docs/iiot/governance/threat-model.md`
- `docs/iiot/governance/control-requirements.md`
- `docs/iiot/governance/publication-policy.md`
- ADRs for exposure, authentication and retention.

## 9. Review criteria
- Every control has an owning stage and a falsifiable check.
- Isolation from physical control is an explicit requirement.
- **Falsified if:** a control relies on a secret stored in Git, or the policy permits redistribution not supported by the source licences.

## 10. Blockers and unverified items
Mark controls that cannot run on the laptop, such as TLS certificate management, as deferred with recorded risk acceptance.

## 11. State, ADR and traceability updates
Update the project state, the ADRs and the traceability table, linking each control to requirements. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): add threat model, control requirements and publication policy`. Do not commit unless the user asks.
- Next prompt: `iiot-08-repository-and-local-infrastructure`.
