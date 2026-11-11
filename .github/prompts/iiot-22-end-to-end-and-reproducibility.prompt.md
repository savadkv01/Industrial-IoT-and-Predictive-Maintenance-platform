---
description: "IIoT Stage 22: validate requirement traceability and end-to-end behaviour from a clean environment, with network-free CI fixtures, opt-in source integration tests and a truthful results record."
agent: "agent"
---

# IIoT Stage 22: End-to-End and Reproducibility

## Read first
Read the following in full and follow it:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/requirements.md`, which contains the traceability table
- `docs/iiot/dev-guide.md`
- every stage's documentation under `docs/iiot/`

## 1. Objective, scope and non-goals
- **Objective:** prove from a clean environment that the platform meets its requirements, and record exactly what was and was not verified.
- **Non-goals:**
  - adding new features;
  - inventing accuracy figures, savings or uptime.

## 2. Prerequisites and inputs
- Stages 01–21 are complete, or deferred with a recorded reason.
- A list of every requirement with a status of `needs revalidation`.

## 3. Domain learning objective
Explain the difference between a **commissioning test**, which checks that the system works as designed, and **performance validation**, which checks that it delivers the business outcome. A portfolio project can usually evidence only the first.

## 4. Decisions to put to the user
1. **CI scope:**
   - (a) GitHub Actions running lint, type checks, unit tests and a network-free tiny end-to-end test on lawful fixtures (recommended);
   - (b) local CI script only;
   - (c) custom.
   Creating the workflow file is allowed; pushing it requires the user's Git request.
2. **Opt-in integration tests:** which source downloads and Compose-based tests should run behind an explicit flag or marker, and never in default CI.
3. **Clean-environment method:** (a) a fresh clone or copy with new volumes on this machine (recommended); (b) a separate WSL distribution or VM; (c) custom.
4. **Demo scenarios:** confirm this list:
   - a tiny demo;
   - an approved measured sample;
   - separate synthetic scenarios;
   - late events;
   - restart;
   - backfill;
   - schema change;
   - quarantine;
   - point-in-time joins;
   - alert deduplication;
   - delayed labels, where implemented.

## 5. Bounded experiment (requires approval)
Run a clean setup and the full scenario suite. Get approval for the runtime and downloads first.

## 6. Gates
Wait for the decisions. Get approval of the plan before creating CI or running long suites.

## 7. Work steps
1. Build an end-to-end test suite organised by scenario. Each test asserts the observable outcomes defined by the requirements.
2. Add a train/inference compatibility test: the scoring code must accept the artifact produced by the training code.
3. Create the minimal CI workflow, if approved, using only network-free fixtures and pinned actions.
4. Execute the clean-environment run. Record:
   - every command;
   - hardware and OS;
   - durations;
   - actual results.
5. Update the traceability table. Give each requirement a status (`verified`, `partially verified`, `unverified` or `deferred`) and a link to its evidence.

## 8. Deliverables
- The E2E tests.
- The CI workflow, if approved.
- `docs/iiot/validation/e2e-report.md`
- An updated traceability table.

## 9. Acceptance checks
- The clean-environment setup succeeds from the documented quickstart alone.
- All default tests pass without network access.
- Every MVP requirement links to evidence or has an explicit gap.
- **Falsified if:**
  - any step needs undocumented manual intervention;
  - CI downloads external data;
  - the report claims accuracy or savings that are not in the measured results.

## 10. Blockers and unverified items
List the unverified criteria explicitly. Do not mark the stage complete while MVP gaps remain unless the user accepts them.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson, then the completion report.

## 12. Handoff
- Proposed commit subject: `test(e2e): add clean-environment scenarios, CI fixtures and validation report`. Do not commit unless the user asks.
- Next prompt: `iiot-23-documentation-and-portfolio-story`.
