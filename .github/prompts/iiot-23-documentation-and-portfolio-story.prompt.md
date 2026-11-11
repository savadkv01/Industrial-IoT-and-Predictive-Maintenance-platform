---
description: "IIoT Stage 23: polish the quickstart, developer guide, data dictionary, source register, glossary, cards, ADR index, runbooks, benchmarks and diagrams against the actual code, and write a truthful portfolio narrative."
agent: "agent"
---

# IIoT Stage 23: Documentation and Portfolio Story

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/validation/e2e-report.md`
- `docs/iiot/governance/publication-policy.md`
- Every document under `docs/iiot/`

## 1. Objective, scope and non-goals
- **Objective:** make the repository understandable, reproducible and credible to a hiring reviewer, with every statement backed by code or evidence.
- **Non-goals:**
  - marketing claims;
  - employer affiliation;
  - production-deployment or downtime-savings claims without evidence;
  - new features.

## 2. Prerequisites and inputs
- The Stage 22 report showing verified and unverified requirements.

## 3. Domain learning objective
Explain how to describe transferable industrial knowledge honestly:
- what the project demonstrates: reliability workflow, measurement semantics, event-time correctness and advisory alerting;
- what would be needed to deploy it in a real plant: OT integration, historian access, safety review and site validation.

## 4. Decisions to put to the user
1. **Root README structure:** offer two or three outlines, recommending this one: problem → what it demonstrates → architecture diagram → quickstart → results with their provenance → limitations → docs map.
2. **Audience emphasis:** (a) a balance of data-engineering depth and industrial domain knowledge (recommended); (b) engineering-heavy; (c) domain-heavy; (d) custom.
3. **Screenshots and demo:** which views to capture, how to sanitise them, and their size limits. Also choose between a recorded GIF and a step-by-step demo script.
4. **Employer relevance wording:** how to mention target UAE industrial employers, if at all. Use generic sector relevance only, with no endorsement or claims about their internal stacks.

## 5. Research or experiment
Check every document against the code. For each command, run it or confirm it ran in the Stage 22 report. Check every path and diagram for drift.

## 6. Gates
Wait for the decisions. Get approval of the documentation plan and the README outline before editing.

## 7. Work steps
1. Update or create the following, and prefer updating existing documents:
   - quickstart;
   - developer guide;
   - data dictionary (generated from the schemas where possible);
   - source register with attribution;
   - domain glossary;
   - measurement and model cards;
   - ADR index;
   - runbooks;
   - benchmarks summary;
   - Mermaid diagrams.
2. Write the portfolio narrative:
   - the senior-level trade-offs, citing the ADRs;
   - the limitations;
   - synthetic, benchmark and measured results kept clearly separated;
   - what would change for production.
3. Add small, lawful, sanitised screenshots that follow the publication policy.
4. Validate the links and the Mermaid syntax.

## 8. Deliverables
- The root `README.md`, if the user approves a shared root artifact.
- The updated `docs/iiot/` documents.
- `docs/iiot/portfolio-story.md`
- The screenshots, at an approved path.

## 9. Review criteria
- Every quantitative claim links to a measured result.
- Every diagram matches the deployed services.
- The quickstart works as written.
- The limitations section is present and specific.
- **Falsified if:**
  - any document claims employer affiliation, production use, certification or savings;
  - synthetic results are presented as real-world accuracy;
  - links are broken.

## 10. Blockers and unverified items
Carry the unverified items from Stage 22 into the limitations section.

## 11. State, ADR and traceability updates
Update the state and the traceability table. Give the domain lesson, then the completion report.

## 12. Handoff
- Proposed commit subject: `docs(iiot): finalize documentation, diagrams and portfolio narrative`. Do not commit unless the user asks.
- Next prompt: `iiot-24-github-release-and-handover`.
