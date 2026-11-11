---
description: "IIoT Stage 15: use Airflow to orchestrate tested entry points for ingestion, backfills, feature recomputation, training and publication, with an explicit streaming lifecycle and proven safe recovery."
agent: "agent"
---

# IIoT Stage 15: Airflow and Recovery

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/architecture/architecture.md`
- The pipeline docs from Stages 10–14: `docs/iiot/pipelines/`, `docs/iiot/features/` and `docs/iiot/alerts/`
- `docs/iiot/dev-guide.md`

## 1. Objective, scope and non-goals
- **Objective:** orchestrate batch work with Airflow, through the package CLI entry points. Make the lifecycle of the continuous streaming runtime explicit, and show that interrupted runs recover safely.
- **Batch work in scope:**
  - historical ingestion;
  - backfills;
  - silver reconciliation;
  - feature recomputation;
  - approved training and evaluation;
  - publication.
- **Non-goals:**
  - one DAG run per sensor event;
  - long-running daemons hidden inside ordinary retrying tasks;
  - domain logic written inside DAG files.

## 2. Prerequisites and inputs
- Tested CLI entry points from Stages 09–14.
- The Airflow service in the Compose `orchestration` profile.

## 3. Domain learning objective
Explain the following:
- why industrial batch recomputation must be idempotent for each partition;
- how backfilling historical periods interacts with alerts. Backfills must not raise alerts that look current.

## 4. Decisions to put to the user
1. **Streaming runtime ownership:**
   - (a) streaming consumers run as Compose services with restart policies and health checks, while Airflow only monitors or reconciles (recommended);
   - (b) Airflow starts and stops the streaming job through a sensor or long-running task, with documented caveats;
   - (c) custom.
2. **DAG set and schedules:** list the proposed DAGs and their cadences, for example daily reconciliation and features, manual backfill and manually triggered training. Get confirmation.
3. **Concurrency and protection:**
   - `max_active_runs=1` on partition-writing DAGs;
   - partition-level locks or atomic overwrite;
   - retries, timeouts and SLAs.
4. **Alerts during backfill:** (a) suppress them, or mark them `historical_backfill` (recommended); (b) raise them normally; (c) custom.

## 5. Research or experiment
- Confirm the pinned Airflow version and its constraints, and whether the TaskFlow API or an operator such as Docker or Bash is best for calling the CLI.

## 6. Gates
Wait for the decisions, then get approval of the plan.

## 7. Work steps
1. Write thin DAGs that call the package CLIs with explicit partitions and run IDs, and that pass no secrets in task arguments.
2. Implement atomic partition writes or MERGE operations so reruns are idempotent.
3. Define checkpoint ownership for each job (streaming checkpoints versus batch watermarks).
4. Write the DAG tests: the import test, cycle and dependency checks, and parameter validation.
5. Recovery demonstration:
   - Kill a backfill mid-run.
   - Rerun it.
   - Show identical gold row counts and hashes, and no duplicate alerts.
   - Also restart the streaming service and confirm it resumes.
6. Write a short orchestration document with the runbook steps.

## 8. Deliverables
- DAG files, any orchestration helpers and the tests.
- `docs/iiot/operations/orchestration.md`
- ADRs for streaming ownership and the backfill alert policy.

## 9. Acceptance checks
- All DAGs import without errors.
- The interrupted-then-rerun backfill gives the same output hash as an uninterrupted run.
- Concurrent triggers do not corrupt the partitions.
- **Falsified if:**
  - domain logic lives in DAG files;
  - reruns duplicate gold records or alerts;
  - the streaming job is silently restarted by Airflow retries without checkpoint ownership.

## 10. Blockers and unverified items
If the Airflow memory exceeds the budget, record it and propose the documented fallback: a plain CLI with cron or the Task Scheduler.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `feat(orchestration): add idempotent Airflow DAGs and recovery demonstration`. Do not commit unless the user asks.
- Next prompt: `iiot-16-quality-and-validation-gates`.
