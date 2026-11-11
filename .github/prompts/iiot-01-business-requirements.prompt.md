---
description: "IIoT Stage 01: agree on users, asset-reliability workflow, business questions, MVP scope and measurable acceptance criteria, with requirement-to-stage traceability."
agent: "agent"
---

# IIoT Stage 01: Business Requirements

## Read first
Read these files in full before doing anything else, and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md) and any accepted ADRs
- [Prompt pack guide](README-iiot.md), to see how later stages use your requirements

## 1. Objective, scope and non-goals
- **Objective:** turn the project brief into agreed, testable requirements for an advisory asset-reliability data platform.
- **In scope:** target users, reliability workflow, business questions, MVP and non-goals, measurable acceptance criteria and a requirement-to-stage traceability table.
- **Non-goals:** choosing data sources, equipment family or technology. Those belong to Stages 03–05. Do not write code or infrastructure.

## 2. Prerequisites and inputs
- None from earlier stages. Confirm that the project state shows Stage 01 as `not started` or `in progress`. If it is `complete`, ask whether this is a revision before you change anything.
- Input: the project brief summarised in the shared contract and this prompt.

## 3. Domain learning objective
Explain the reliability workflow as **detect → diagnose → prioritise inspection → maintain → learn**. Then explain why these four capabilities are separate, with separate evidence needs:
- platform reliability, meaning the data arrives correctly and on time;
- anomaly detection;
- current fault diagnosis;
- future failure prediction.

## 4. Decisions to put to the user (2–4 per interaction)
1. **Primary user persona:** (a) reliability engineer (recommended: needs evidence and trends); (b) maintenance planner (needs a prioritised queue); (c) plant data/platform engineer (needs pipeline health); (d) custom.
2. **Headline business questions for the MVP.** Offer a shortlist and ask the user to pick 2–4, for example: which assets are in an abnormal operating condition now; which component condition is currently degraded; which assets should be inspected first, and why; whether the telemetry is trustworthy and fresh.
3. **Ambition for prediction:** (a) monitoring + diagnosis in the MVP, forecasting as an optional benchmark (recommended, because longitudinal labels are scarce); (b) forecasting as a core MVP goal, subject to source evidence; (c) no forecasting; (d) custom.
4. **Acceptance-criteria style:** (a) measurable platform SLOs plus analytical criteria to be set after the data is profiled (recommended); (b) fixed numeric model targets now (not recommended before profiling); (c) custom.
5. **Advisory boundary wording:** confirm that outputs are inspection recommendations only, with no control actions or work-order automation.
6. **Project working name:** keep the provisional name or choose another.

Recommend options, explain the trade-offs and wait for answers.

## 5. Research or experiment
No experiment is needed. If the user asks how relevant a requirement is to target employers, use only public, general industry knowledge. Never claim anything about an employer's internal systems.

## 6. Gates
Wait for every answer. Then propose the document outline and traceability approach, and get approval before writing.

## 7. Work steps
1. Draft personas, the workflow and the business questions.
2. Write requirements with IDs, grouped as functional (`BR-F-nn`), analytical (`BR-A-nn`), platform/non-functional (`BR-N-nn`) and constraints (`BR-C-nn`). Mark each one *MVP* or *optional*.
3. For each requirement, write an acceptance criterion and a falsification test, or `TBD after Stage 03/04` where the numbers depend on data.
4. List the non-goals explicitly: physical control, safety certification, production OT compliance and quantified savings claims.
5. Add a traceability table mapping each requirement to its implementing stages and verifying stages.

## 8. Deliverables
- `docs/iiot/requirements.md`, owned by Stage 01 and updated by later stages through traceability changes only.
- ADRs for persona/MVP scope and prediction ambition, if the user treats them as material.

## 9. Acceptance and review criteria
- Every requirement is testable or explicitly deferred with a reason. No numeric model-accuracy target is invented before profiling.
- Platform reliability, anomaly detection, diagnosis and prediction are separate requirements.
- Every requirement maps to at least one stage between 02 and 24.
- **Falsified if:** any requirement implies control actions, employer endorsement, or predictive claims that no data source could support.

## 10. Blockers and unverified items
List the assumptions that need source evidence, such as whether usable failure labels exist. Leave them unverified until Stage 03.

## 11. State, ADR and traceability updates
Update the project state (status, approved requirements, unresolved questions) and the ADR index. Finish with the completion report defined in the contract.

## 12. Handoff
- Proposed commit subject: `docs(iiot): define business requirements and traceability`. Do not commit unless the user asks.
- Next prompt to run manually: `iiot-02-industrial-domain-and-measurement`.
