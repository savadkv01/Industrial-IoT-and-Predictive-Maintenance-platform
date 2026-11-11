---
description: "IIoT Stage 18: add correlated logs and metrics for pipeline, data and service health, actionable local alerts and tested runbooks, using optional monitoring profiles."
agent: "agent"
---

# IIoT Stage 18: Observability and Runbooks

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/architecture/architecture.md`
- `docs/iiot/pipelines/`
- `docs/iiot/operations/orchestration.md`
- `docs/iiot/quality/quality-contracts.md`
- `docs/iiot/serving/api.md`

## 1. Objective, scope and non-goals
- **Objective:** make the platform's health observable, and make every failure state actionable through tested runbooks.
- **Non-goals:**
  - enterprise observability stacks;
  - paging integrations;
  - pretending a laptop provides continuous uptime.

## 2. Prerequisites and inputs
- The monitoring-profile decision from Stage 05.
- Running services from Stages 10–17.

## 3. Domain learning objective
Explain the following:
- **Event-time lag and wall-clock lag:** why the two diverge under accelerated replay;
- **Laptop sleep:** why it appears as downtime rather than as a data gap at the asset.

## 4. Decisions to put to the user
1. **Stack:**
   - (a) Prometheus, Grafana and structured JSON logs with correlation and run IDs, under the `monitoring` profile (recommended if it was approved in Stage 05);
   - (b) logs plus a CLI health report only;
   - (c) custom.
2. **Signals.** Propose this list and confirm it:
   - broker consumer lag;
   - throughput;
   - event-time freshness per asset class;
   - late, dropped and quarantined counts;
   - checkpoint age;
   - quality-gate outcomes;
   - alert volume;
   - API latency and errors;
   - container memory and disk.
   Avoid high-cardinality labels such as event IDs.
3. **Local alert rules and thresholds:** propose 4–6 actionable rules, each with its runbook.
4. **Laptop downtime:** annotate gaps in the replay or wall clock instead of alerting on them.

## 5. Research or experiment
- Confirm the exporters available for the chosen broker and database, and their memory cost.
- Measure the RAM used by the monitoring profile.

## 6. Gates
Wait for the decisions, then get approval of the plan.

## 7. Work steps
1. Add structured logging with run IDs and correlation IDs across the services, plus secret redaction. Test the redaction immediately.
2. Expose metrics from the package services, and configure exporters for the infrastructure.
3. Provision the dashboards and alert rules as code.
4. For each alert rule, write a runbook covering the symptom, likely causes, diagnostic commands, safe remediation and verification.
5. Fire-drill: trigger at least one alert (for example, stop the consumer to grow the lag), follow the runbook and verify the recovery.

## 8. Deliverables
- Logging and metrics code, provisioning files and the Compose `monitoring` profile.
- `docs/iiot/operations/observability.md`
- `docs/iiot/operations/runbooks/*.md`
- ADRs.

## 9. Acceptance checks
- The fire-drill alert fires and then resolves after the runbook steps.
- Log samples contain no secrets.
- The metric series count stays within the agreed bound.
- The monitoring profile stays within its RAM budget.
- **Falsified if:** an alert has no runbook, wall-clock lag is presented as event-time freshness, or the monitoring stack is required to run the core demo.

## 10. Blockers and unverified items
Mark any alert that was not fire-drilled as `untested`.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(observability): add metrics, local alert rules and tested runbooks`. Do not commit unless the user asks.
- Next prompt: `iiot-19-model-lifecycle-and-drift`.
