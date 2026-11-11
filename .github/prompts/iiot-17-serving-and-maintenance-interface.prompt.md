---
description: "IIoT Stage 17: serve asset health, telemetry trends, data quality, provenance, scores and the inspection queue through a minimal API and dashboard, with visible units, freshness and provenance and an audited acknowledgement."
agent: "agent"
---

# IIoT Stage 17: Serving and Maintenance Interface

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/requirements.md`, specifically the personas and business questions
- `docs/iiot/alerts/alert-lifecycle.md`
- `docs/iiot/quality/quality-contracts.md`
- `docs/iiot/governance/control-requirements.md`

## 1. Objective, scope and non-goals
- **Objective:** build a minimal, trustworthy interface that answers the Stage 01 business questions.
- **Non-goals:**
  - certified maintenance guidance;
  - control actions;
  - building a full CMMS;
  - public internet exposure.

## 2. Prerequisites and inputs
- Gold features, scores, the alert queue and the quality results in the serving store.
- The authentication decisions from Stage 07.

## 3. Domain learning objective
Explain what reliability engineers need in order to trust an alert:
- the raw trend with its units;
- the operating regime;
- data freshness and quality;
- the model version;
- comparable past episodes.

## 4. Decisions to put to the user
1. **Stack:**
   - (a) FastAPI with a small server-rendered or Streamlit dashboard (recommended for speed);
   - (b) FastAPI plus Grafana panels over Postgres;
   - (c) a FastAPI and React SPA, which takes more effort;
   - (d) custom.
2. **Views for the MVP:**
   - fleet health overview;
   - asset detail with trends and regime;
   - inspection queue with evidence;
   - data-quality and freshness view;
   - provenance and model info.
   Pick and order the views.
3. **Acknowledgement and roles:** who may acknowledge and change states, and whether a comment is required. Confirm that the audit captures the actor and time.
4. **Uncertainty display:** score bands or intervals, and wording that avoids implying a probability without calibration.

## 5. Research or experiment
None required beyond confirming the library versions.

## 6. Gates
Wait for the decisions. Get approval of the API contract (OpenAPI outline) and the wireframe list before you code.

## 7. Work steps
1. Define the API contract:
   - versioned routes;
   - pagination and filtering by asset, state, severity and provenance track;
   - responses that include units, `observed_at`, `as_of_time`, freshness, `provenance_track`, `model_version` and uncertainty.
2. Implement the read endpoints, and the acknowledgement or state-change endpoints with authorization and audit. Test the first endpoint immediately.
3. Implement the dashboard views. Show measured, replayed and synthetic data with distinct, labelled badges. Show `insufficient_data` and `quality_blocked` states clearly.
4. Add API contract tests and authorization tests that show a denied operation.
5. Add UI smoke tests that render the pages on desktop and mobile viewports with non-blank content. Use a browser tool if one is available.

## 8. Deliverables
- API and dashboard modules, tests and the Compose service.
- `docs/iiot/serving/api.md`
- `docs/iiot/serving/dashboard.md`
- ADRs.

## 9. Acceptance checks
- The contract tests pass.
- An unauthorised acknowledgement is denied and the attempt is audited.
- Every displayed value has its unit and timestamp.
- The provenance badges are present.
- The pages render with non-blank content at both viewport sizes.
- **Falsified if:** synthetic data is shown without a label, stale data appears as current, or any UI action implies equipment control.

## 10. Blockers and unverified items
Record any accessibility and usability items that have not been evaluated.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(serving): add asset health API and advisory inspection dashboard`. Do not commit unless the user asks.
- Next prompt: `iiot-18-observability-and-runbooks`.
