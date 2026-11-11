# IIoT Prompt Pack — Industrial Asset Reliability & Predictive Maintenance

The prompt pack splits the industrial IoT portfolio platform into 24 stages, one prompt each. Each stage runs in its own Agent chat and stops at its approval gates. No stage starts the next one.

## How to run a stage

1. Open a new Copilot Agent chat in VS Code.
2. Type `/` and choose the stage prompt, for example `/iiot-01-business-requirements`. Alternatively, run **Chat: Run Prompt...** from the Command Palette and pick the file.
3. Answer the decision questions and approve, or change, the implementation plan.
4. Review the diffs and the completion report. Commit and merge only if you choose to; the agent will ask before running any Git operation.
5. Accept the stage outcome so it can be marked `complete` in [project-state.md](../../docs/iiot/project-state.md). Then launch the next stage manually.

**Resuming:** start a fresh chat with the same stage prompt. The agent rebuilds context from [project-state.md](../../docs/iiot/project-state.md), the [ADRs](../../docs/iiot/decisions/README.md) and the files in the repository, not from earlier chats.

## Shared files

| File | Purpose |
| --- | --- |
| [iiot-workflow.instructions.md](../instructions/iiot-workflow.instructions.md) | Interaction, decision, truthfulness, safety and Git contract. Every stage reads it. |
| [project-state.md](../../docs/iiot/project-state.md) | Stage status, approvals, evidence, blockers and next action |
| [decisions/README.md](../../docs/iiot/decisions/README.md) | ADR template and index |

## Selected workflow policies

- **Granularity:** 24 stages.
- **Learning:** guided explanations, with approval at each material decision.
- **Git:** user-initiated only. Stages propose a truthful commit subject and file list. The agent runs `git` commit, merge, push or tag only when you explicitly ask for that specific operation.

## Behavioral guidance vs. tool permissions

These prompts only guide the agent; they cannot technically block tool use. Keep VS Code tool-approval prompts enabled, especially for shell, network and Git commands, and review every diff before accepting it.

## Sequence, dependencies and outputs

Effort figures are **rough estimates** for a senior data engineer learning the domain. Actual effort depends on your hardware, the sources you choose and how deep you go.

| # | Prompt | Depends on | Key outputs (created by that stage) | Decision checkpoints | Est. effort |
| --- | --- | --- | --- | --- | --- |
| 01 | [business-requirements](iiot-01-business-requirements.prompt.md) | — | `docs/iiot/requirements.md` + traceability | users, MVP, acceptance criteria | 0.5–1 d |
| 02 | [industrial-domain-and-measurement](iiot-02-industrial-domain-and-measurement.prompt.md) | 01 | glossary, measurement & label contracts | failure/label definitions, reliability metrics | 1–1.5 d |
| 03 | [source-discovery-and-profiling](iiot-03-source-discovery-and-profiling.prompt.md) | 01, 02 | source register, coverage & claim matrices, profiles | which sources to sample, licence acceptance | 1–2 d |
| 04 | [equipment-scope-and-feasibility](iiot-04-equipment-scope-and-feasibility.prompt.md) | 03 | feasibility report, go/no-go | equipment family, data tracks, budgets | 0.5–1 d |
| 05 | [solution-and-data-architecture](iiot-05-solution-and-data-architecture.prompt.md) | 04 | architecture doc, ADRs, Mermaid diagrams | transport, storage, compute, profiles | 1–2 d |
| 06 | [data-modelling-and-contracts](iiot-06-data-modelling-and-contracts.prompt.md) | 05 | data model, contracts, mappings | grains, time semantics, versioning | 1–2 d |
| 07 | [governance-and-security-design](iiot-07-governance-and-security-design.prompt.md) | 05, 06 | threat model, control requirements, publication policy | exposure, auth, retention | 0.5–1 d |
| 08 | [repository-and-local-infrastructure](iiot-08-repository-and-local-infrastructure.prompt.md) | 05–07 | package skeleton, Compose, CI-ready tooling | layout, pins, profiles | 1–2 d |
| 09 | [replay-and-synthetic-fleet](iiot-09-replay-and-synthetic-fleet.prompt.md) | 06, 08 | replay adapters, seeded simulator | scenarios, rates, assumptions | 2–3 d |
| 10 | [ingestion-and-bronze](iiot-10-ingestion-and-bronze.prompt.md) | 09 | bronze ingestion, quarantine, recovery test | envelopes, retries, bridge | 2–3 d |
| 11 | [event-time-and-silver](iiot-11-event-time-and-silver.prompt.md) | 10 | silver transforms, watermarks, reconciliation | lateness bound, dedup, units | 2–3 d |
| 12 | [point-in-time-features-and-gold](iiot-12-point-in-time-features-and-gold.prompt.md) | 11 | feature library, gold tables, leakage tests | windows, resets, freshness | 2–3 d |
| 13 | [baselines-and-model-evaluation](iiot-13-baselines-and-model-evaluation.prompt.md) | 12 | baselines, evaluation protocol & results | splits, metrics, horizon | 2–4 d |
| 14 | [scoring-and-alert-lifecycle](iiot-14-scoring-and-alert-lifecycle.prompt.md) | 13 | scoring service, inspection queue | thresholds, hysteresis, states | 1.5–2.5 d |
| 15 | [airflow-and-recovery](iiot-15-airflow-and-recovery.prompt.md) | 10–14 | DAGs, recovery demonstration | DAG scope, streaming lifecycle | 1.5–2.5 d |
| 16 | [quality-and-validation-gates](iiot-16-quality-and-validation-gates.prompt.md) | 10–14 | consolidated quality contracts, fault injection | warn/fail/quarantine policy | 1–2 d |
| 17 | [serving-and-maintenance-interface](iiot-17-serving-and-maintenance-interface.prompt.md) | 14, 16 | API + dashboard | UI stack, views, audit | 2–3 d |
| 18 | [observability-and-runbooks](iiot-18-observability-and-runbooks.prompt.md) | 10–17 | metrics, alerts, runbooks | signals, thresholds | 1–2 d |
| 19 | [model-lifecycle-and-drift](iiot-19-model-lifecycle-and-drift.prompt.md) | 13, 14 | versioning, promotion/rollback, drift monitors | registry, promotion rules | 1.5–2.5 d |
| 20 | [security-and-governance-verification](iiot-20-security-and-governance-verification.prompt.md) | 07, 08–19 | verification report, scans | residual risk acceptance | 1–1.5 d |
| 21 | [performance-and-resource-finops](iiot-21-performance-and-resource-finops.prompt.md) | 10–19 | benchmarks, guardrails | workloads, budgets | 1–2 d |
| 22 | [end-to-end-and-reproducibility](iiot-22-end-to-end-and-reproducibility.prompt.md) | 01–21 | E2E report, CI fixtures | scope of CI vs opt-in tests | 1–2 d |
| 23 | [documentation-and-portfolio-story](iiot-23-documentation-and-portfolio-story.prompt.md) | 22 | polished docs, diagrams, demo narrative | narrative, screenshots | 1–2 d |
| 24 | [github-release-and-handover](iiot-24-github-release-and-handover.prompt.md) | 23 | release checklist, notes, backlog | code licence, publication | 0.5–1 d |

Quality, security, governance, testing and documentation start in the first stage that touches them. Stages 16, 18, 20, 22 and 23 consolidate and verify that work; they do not introduce it for the first time.

## Revisiting decisions

If a later stage changes an accepted decision, a new ADR supersedes the old one. Downstream stages are then marked `needs revalidation` in project state. Re-run those stage prompts to revalidate them.
