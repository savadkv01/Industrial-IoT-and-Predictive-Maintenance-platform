# IIoT Project State

Working name: *Industrial Asset Reliability & Predictive Maintenance Data Platform* (provisional; may change in Stage 01).

Shared contract: [iiot-workflow.instructions.md](../../.github/instructions/iiot-workflow.instructions.md) · Decisions: [decisions/README.md](decisions/README.md) · Prompt guide: [README-iiot.md](../../.github/prompts/README-iiot.md)

## Workflow preferences (not architecture approvals)

| Preference | Selection |
| --- | --- |
| Granularity | 24 stages |
| Learning style | Guided explanations with approval at material decisions |
| Git policy | User-initiated only: commit and merge only when the user explicitly asks; stages propose commit subjects |
| Repository arrangement | Single project in this workspace (no other project detected at generation) |
| Git repository | Initialised on `main`; remote `origin` = https://github.com/savadkv01/Industrial-IoT-and-Predictive-Maintenance-platform |

## Active stage

None. Stage 01 is complete and accepted. Next action: run `iiot-02-industrial-domain-and-measurement` manually.

## Stage status

| # | Stage | Status | Evidence / outputs | Notes |
| --- | --- | --- | --- | --- |
| 01 | Business requirements | complete | `requirements.md`; ADRs 0001–0002 | Accepted by user; numeric platform SLO targets deferred. |
| 02 | Industrial domain & measurement | not started | — | |
| 03 | Source discovery & profiling | not started | — | |
| 04 | Equipment scope & feasibility | not started | — | |
| 05 | Solution & data architecture | not started | — | |
| 06 | Data modelling & contracts | not started | — | |
| 07 | Governance & security design | not started | — | |
| 08 | Repository & local infrastructure | not started | — | |
| 09 | Replay & synthetic fleet | not started | — | |
| 10 | Ingestion & bronze | not started | — | |
| 11 | Event time & silver | not started | — | |
| 12 | Point-in-time features & gold | not started | — | |
| 13 | Baselines & model evaluation | not started | — | |
| 14 | Scoring & alert lifecycle | not started | — | |
| 15 | Airflow & recovery | not started | — | |
| 16 | Quality & validation gates | not started | — | |
| 17 | Serving & maintenance interface | not started | — | |
| 18 | Observability & runbooks | not started | — | |
| 19 | Model lifecycle & drift | not started | — | |
| 20 | Security & governance verification | not started | — | |
| 21 | Performance & resource FinOps | not started | — | |
| 22 | End-to-end & reproducibility | not started | — | |
| 23 | Documentation & portfolio story | not started | — | |
| 24 | GitHub release & handover | not started | — | |

## Approved requirements and decisions

- Primary persona: reliability engineer.
- MVP questions: abnormal operating condition now; currently degraded component; inspection priority and rationale; telemetry trustworthiness and freshness.
- MVP ambition: monitoring and diagnosis; forecasting is an optional benchmark subject to source evidence.
- Acceptance style: analytical criteria follow profiling; numeric platform SLO targets are deferred pending source/feasibility/architecture evidence.
- Advisory boundary: inspection recommendations only; no control actions or automated work orders.
- Working name: Industrial Asset Reliability & Predictive Maintenance Data Platform.
- Stage 01 requirements and deliverables accepted by the user. Numeric platform SLO targets remain deferred. See [decisions/README.md](decisions/README.md).

## Checks run

- Stage 01 documentation/traceability consistency review: passed; all 12 requirements have acceptance and falsification criteria and map to implementation and verification stages.

## Blockers

None recorded.

## Unresolved questions

- Whether the selected data can support component-level diagnosis or future-failure labels (Stage 03).
- Source coverage, quality, time semantics, licensing and redistribution terms (Stage 03).
- Numeric platform SLO targets and analytical thresholds (Stages 03–05).
- Equipment family, data tracks, architecture and hardware budget (Stages 02–05).

## Deferred optional capabilities

None recorded.

## Change log

| Date | Stage | Change |
| --- | --- | --- |
| 2026-11-11 | 00 | Workflow generated; all stages initialised as not started. |
| 2026-11-11 | 00 | Git initialised on user request; workflow files committed (`1f224c5`) and pushed to `origin/main`. |
| 2026-11-11 | 01 | Recorded user selections; completed and accepted business requirements and ADRs 0001–0002. Numeric platform SLO targets deferred. |
