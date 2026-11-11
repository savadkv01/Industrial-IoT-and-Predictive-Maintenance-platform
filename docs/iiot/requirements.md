# Business Requirements

- Status: Stage 01 complete; accepted by user
- Working name: Industrial Asset Reliability & Predictive Maintenance Data Platform
- Scope: advisory asset-reliability data platform; no equipment family, source or technology selected

## Purpose and users

The primary user is a **reliability engineer**, who needs evidence and trends to investigate equipment condition and plan inspections. The MVP supports the reliability workflow:

**detect → diagnose → prioritise inspection → maintain → learn**

These are separate steps. Platform reliability measures whether data arrives correctly and on time; anomaly detection flags unusual behaviour; current fault diagnosis identifies a possibly degraded component; future failure prediction estimates a later event. Evidence for one capability does not establish another.

## MVP questions and scope

The MVP is intended to answer:

1. Which assets are in an abnormal operating condition now?
2. Which component condition is currently degraded?
3. Which assets should be inspected first, and why?
4. Is telemetry trustworthy and fresh?

Monitoring and current diagnosis are in MVP scope. Forecasting is an **optional benchmark**, contingent on defensible source labels and evaluation evidence; it is not an MVP acceptance requirement. Numeric analytical targets and numeric platform SLO targets are deferred until source profiling and feasibility/architecture evidence are available. No target is implied by this draft.

## Requirements

Each requirement is marked MVP or optional and has a testable acceptance criterion. Where data evidence is required, the criterion states what must be established rather than inventing a target.

| ID | Type / scope | Requirement | Acceptance criterion | Falsification test |
| --- | --- | --- | --- | --- |
| BR-F-01 | Functional / MVP | Present evidence indicating whether an asset is currently in an abnormal operating condition. | A supported demonstration identifies the asset, relevant observation window, signal/feature evidence and data availability time. | The result cannot be traced to observations available by the stated decision time, or reports an anomaly without its evidence/window. |
| BR-F-02 | Functional / MVP | Present evidence for a currently degraded component when the selected data supports component-level diagnosis. | The demonstration identifies the component and supporting observations or explicitly reports that component-level diagnosis is unsupported. | It assigns a component or fault without evidence in the selected data, or presents an anomaly as a confirmed fault. |
| BR-F-03 | Functional / MVP | Help the reliability engineer prioritise assets for inspection and explain why. | A demonstration produces an ordered or grouped inspection view with the evidence and rationale visible for each included asset. | The order cannot be explained from displayed evidence, or is represented as an automated maintenance action. |
| BR-F-04 | Functional / MVP | Make telemetry trustworthiness and freshness visible to users. | A demonstration exposes the applicable quality, completeness, freshness and provenance information; thresholds that depend on source evidence are explicitly documented. | Missing, stale or invalid telemetry is silently presented as trustworthy and current. |
| BR-A-01 | Analytical / MVP | Keep anomaly detection distinct from fault diagnosis and future failure prediction. | User-facing terminology and evaluation evidence identify anomaly outputs as indicators of unusual behaviour, not confirmed faults or failure probabilities. | An anomaly score or threshold is presented as a diagnosis or calibrated probability without supporting validation. |
| BR-A-02 | Analytical / MVP | Make current fault/component diagnosis only to the level supported by evidence. | Evaluation distinguishes supported component diagnoses from unknown/unsupported cases and reports the evidence and limitations. | A diagnosis is asserted where labels or observations cannot support it, or unknown cases are silently treated as healthy. |
| BR-A-03 | Analytical / Optional | Explore future failure prediction only as an optional benchmark, if suitable labels and follow-up are available. | Any benchmark states its horizon, label provenance, observation availability, censoring treatment and leakage controls; otherwise it is recorded as infeasible or deferred. | A forecast is claimed without defensible future labels, or insufficient follow-up is treated as a negative label. |
| BR-N-01 | Platform / MVP | Measure data delivery and processing reliability separately from analytical performance. | The implementation reports measurable delivery/freshness indicators and a declared test method. Numeric SLO targets remain TBD pending source profiling and feasibility/architecture evidence. | No reproducible measurement can show whether data was accounted for and available within the declared test window, or analytics are used as a substitute for pipeline-health evidence. |
| BR-N-02 | Platform / MVP | Preserve sufficient provenance and time semantics to interpret evidence. | Demonstrations distinguish source-observation, cycle-relative, assigned replay, ingestion, processing and availability times where applicable, and identify data lineage. | Replay time is described as observation time, row/cycle order as continuous machine history, or evidence cannot be traced to its source and processing. |
| BR-C-01 | Constraint / MVP | Keep all outputs advisory: inspection recommendations only, with no control actions or automated work orders. | Product and documentation review confirms that outputs do not issue commands to equipment or create/dispatch work orders. | Any feature directly changes equipment state or automates work-order execution. |
| BR-C-02 | Constraint / MVP | Separate measured experimental data, externally simulated benchmarks and custom synthetic-fleet data, and bound claims to their evidence. | Every demonstration and result labels its data track and states what it can and cannot establish. | Tracks are blended into an apparently real asset history, or replay/synthetic results are claimed as proof of physical fidelity, safety or industrial predictive accuracy. |
| BR-C-03 | Constraint / MVP | Avoid unsupported claims of safety certification, production OT compliance, employer endorsement or quantified savings. | Public-facing project claims are traceable to verified evidence and omit these claims unless independently established and explicitly in scope. | Any such claim appears without appropriate evidence and approval. |

## Non-goals

- Physical control, actuator integration, or automated work-order creation/dispatch.
- Safety certification or production OT compliance.
- Claims of quantified savings, uptime, predictive accuracy or industrial readiness without supporting evidence.
- Selection of equipment family, data sources, technology or architecture; these belong to later stages.

## Assumptions and evidence needed

- Availability and suitability of component-level fault labels are unverified until source discovery and profiling (Stage 03).
- Availability of defensible future-failure labels, observation windows and follow-up is unverified; insufficient follow-up must not be interpreted as a negative label.
- Source coverage, data quality, time semantics and license/redistribution terms are unverified.
- Numeric analytical criteria and numeric platform SLO targets are deferred; establish them only after relevant source and feasibility/architecture evidence (Stages 03–05).
- The selected MVP questions are user-approved scope, not a claim that all questions are answerable by a future source.

## Requirement-to-stage traceability

“Implementing” identifies planned stages that define or deliver the capability; “verifying” identifies planned stages that test or review it. These are plans, not evidence that a stage has run.

| Requirement | Implementing stages | Verifying stages |
| --- | --- | --- |
| BR-F-01 | 02, 11, 12, 13, 14 | 13, 16, 22 |
| BR-F-02 | 02, 06, 12, 13, 14 | 13, 16, 22 |
| BR-F-03 | 12, 13, 14, 17 | 14, 17, 22 |
| BR-F-04 | 06, 10, 11, 16, 17 | 11, 16, 18, 22 |
| BR-A-01 | 02, 12, 13, 14 | 13, 16, 22 |
| BR-A-02 | 02, 06, 12, 13, 14 | 13, 16, 22 |
| BR-A-03 | 02, 03, 12, 13, 19 | 13, 19, 22 |
| BR-N-01 | 08, 10, 11, 16, 18, 21 | 16, 18, 21, 22 |
| BR-N-02 | 03, 06, 10, 11 | 16, 20, 22 |
| BR-C-01 | 07, 14, 17, 20 | 20, 22 |
| BR-C-02 | 03, 09, 13, 20, 23 | 20, 22 |
| BR-C-03 | 07, 20, 23 | 20, 23 |

Update this matrix as later stages define or verify the requirements. Changes to agreed scope require a new user decision and, when material, an ADR.
