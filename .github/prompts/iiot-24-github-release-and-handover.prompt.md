---
description: "IIoT Stage 24: review public readiness, confirm the code licence separately from data terms, propose repository metadata and truthful release notes, and deliver an evidence checklist and prioritised backlog. Publication only on explicit authorization."
agent: "agent"
---

# IIoT Stage 24: GitHub Release and Handover

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/governance/verification-report.md`
- `docs/iiot/governance/publication-policy.md`
- `docs/iiot/validation/e2e-report.md`
- `docs/iiot/portfolio-story.md`

## 1. Objective, scope and non-goals
- **Objective:** confirm that the repository is ready to publish, prepare the release material, and hand over a checklist of requirements against evidence plus a prioritised backlog.
- **Non-goals:**
  - creating repositories, pushing, tagging or publishing releases without the user explicitly requesting each operation;
  - rewriting, squashing or backdating history.

## 2. Prerequisites and inputs
- Stages 20, 22 and 23 complete, or their gaps accepted by the user.

## 3. Domain learning objective
Explain the difference between licensing the *code*, honouring the *data* terms (attribution, and redistribution limits that may apply), and *model artifacts* derived from the data.

## 4. Decisions to put to the user
1. **Code licence**, asked separately from the data terms: (a) MIT; (b) Apache-2.0 (recommended for its explicit patent grant); (c) custom. Explain that the code licence does not relicense third-party data.
2. **Data and model artifact publication:**
   - (a) publish fixtures only, with attribution, and provide download scripts for the full sources (recommended);
   - (b) include derived models, if the source terms allow it;
   - (c) custom.
3. **Repository metadata:** name, description, topics and visibility. Propose truthful wording.
4. **Publication method:**
   - (a) the user runs the Git and GitHub steps manually from the checklist provided (recommended under the user-initiated Git policy);
   - (b) the user explicitly asks the agent to run specific `git` or `gh` commands one at a time;
   - (c) defer.

## 5. Research or experiment
Run a final read-only readiness review covering:
- large files;
- ignored paths;
- secrets (rerun the Stage 20 scanners if available);
- licence files and attribution;
- CI status;
- broken links.

If Git exists, review the commit history for genuine increments. Report any problems, but do not rewrite.

## 6. Gates
Wait for each decision. Get approval of the release plan. Run any Git or GitHub operation only on an explicit user request for that operation.

## 7. Work steps
1. Add the approved `LICENSE` file and a `NOTICE` or attribution section for data and dependencies.
2. Draft the repository description, topics and truthful release notes. The notes cover what works, verified results with their provenance, known limitations and the backlog.
3. Write a short portfolio summary of 3–5 sentences.
4. Produce a requirement/evidence checklist from the traceability table.
5. Produce a prioritised backlog: deferred optional capabilities, unverified requirements and production-hardening items.
6. Give manual publication steps, for example `git remote add`, `git push` or `gh release create`, for the user to run. Run them only if the user explicitly asks, and never push secrets or large data.

## 8. Deliverables
- `LICENSE` and the attribution notices
- `docs/iiot/release/release-notes.md`
- `docs/iiot/release/release-checklist.md`
- `docs/iiot/release/backlog.md`
- The final project state

## 9. Acceptance checks
- The readiness review finds no secrets, no unapproved large files and no missing attribution.
- Every release claim links to evidence.
- The licence choice is recorded in an ADR.
- **Falsified if:**
  - a publication action happened without an explicit request;
  - the release notes claim unverified results;
  - history was rewritten.

## 10. Blockers and unverified items
If `gh` or authentication is unavailable, provide manual web UI steps. Never request tokens in chat.

## 11. State, ADR and traceability updates
- Mark the final statuses honestly.
- Record the licence and publication ADRs.
- Close the project state with the next recommended actions.
- Give the final domain lesson and the completion report.

## 12. Handoff
- Proposed commit subject: `docs(release): add licence, attribution, release notes and handover checklist`. Do not commit unless the user asks.
- Next step: none. The workflow is complete. Suggest revisiting any items marked `needs revalidation` or the backlog.
