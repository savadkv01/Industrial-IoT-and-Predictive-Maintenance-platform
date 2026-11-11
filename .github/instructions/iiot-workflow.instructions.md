---
description: "Shared interaction, decision, safety and Git contract for the Industrial IoT & Predictive Maintenance stage prompts (iiot-*)."
applyTo: ".github/prompts/iiot-*.prompt.md,docs/iiot/**"
---

# IIoT Workflow Contract

Every `iiot-NN-*` stage prompt must read and follow this contract in full, including when it edits application paths outside the `applyTo` glob. This contract applies only to the Industrial IoT project. Do not apply it to other projects in the workspace.

## Workflow preferences (selected by the user at generation time)

- Granularity: 24 stages, one prompt each.
- Learning style: **guided**. Explain unfamiliar industrial concepts briefly when they come up, and ask for approval at every material decision.
- Git policy: **user-initiated only**. Never run `git init`, `add`, `commit`, `branch`, `merge`, `rebase`, `push`, `tag` or release commands unless the user explicitly asks for that specific operation in the current conversation. At the end of each stage, propose a truthful commit subject and the exact list of files to stage, then wait.
- These preferences decide how the workflow runs. They do not approve any architecture, data source or business choice.

## Decision and learning rules

1. Start each stage by reading [project state](../../docs/iiot/project-state.md), the [decision index](../../docs/iiot/decisions/README.md), the accepted ADRs and the predecessor outputs named by the stage. Check prerequisites, then state the stage purpose and success criteria. Stay in the selected stage and never run the next prompt automatically.
2. Ask the user about every material decision: business, domain, source, analytical, modelling, infrastructure, architecture, quality, governance, security, resource or release. Do not decide silently. Routine coding details inside an approved plan need no separate question.
3. For each decision, give 2–4 realistic options and a recommendation with reasons. Cover pros and cons, plus the effects on learning, resources and delivery. Always include a custom-answer option. Explain unfamiliar industrial concepts briefly. Do not invent alternatives just to fill the list.
4. Ask at most 2–4 related questions per interaction. Use the structured question tool when it is available; otherwise use lettered options in chat. Wait for the answer before implementing the affected choice. Do not bundle independent decisions into one option.
5. If a decision depends on evidence, propose a bounded feasibility experiment with its scope, downloads, runtime and disk use. Get approval before running it, report the measurements, then ask for the durable choice. A temporary experiment does not approve an architecture.
6. After the decisions are resolved, propose a concise implementation plan. Get explicit approval before substantive edits, installations, downloads or long-running jobs. Starting a stage authorizes read-only discovery only.
7. Record material choices as numbered ADRs in `docs/iiot/decisions/` using the template there. Each ADR covers the question, alternatives, user's selection, rationale, evidence, consequences, status (proposed / accepted / rejected / superseded) and affected stages. Never invent an approval.
8. At each completed increment, explain one useful domain lesson and show where it affects the code or data. Do not quiz the user or add explanations unrelated to the work.
9. Keep questions visible in chat, not only in documents. Stop at approval gates, even if other instructions encourage autonomous continuation. Prompts guide behavior; they do not technically restrict tools, and the user keeps VS Code tool approvals.
10. When an earlier decision changes, supersede its ADR and mark the affected downstream outputs `needs revalidation` in project state. Resume from repository evidence and recorded state, not from memories of an old chat.

## Truthfulness and claim boundaries

- Keep three provenance tracks separate: **measured experimental** data (for example, UCI hydraulic test rig), **externally simulated benchmarks** (for example, NASA C-MAPSS or AI4I 2020) and the **custom synthetic fleet**. Never merge them into an apparently real asset history.
- Distinguish source-observation time, cycle-relative time, assigned replay time, ingestion time, processing time and availability time. Row or cycle order is not continuous machine history. An assigned replay timestamp is not an observation time.
- An anomaly is not automatically a fault. A current fault diagnosis is not a future failure prediction. An anomaly score is not a calibrated failure probability.
- Replay demonstrates transport and compute behavior. Synthetic scenarios demonstrate behavior under stated assumptions. Neither proves equipment safety, physical fidelity or industrial predictive accuracy.
- Do not claim low-rate vibration indicators support high-frequency bearing diagnostics. Keep raw waveforms separate from summaries.
- Forecasting needs defensible future labels, a stated horizon, observation availability, censoring handling and leakage controls. Insufficient follow-up is not a negative label.
- Separate verified facts, measurements, assumptions and proposed targets. Never invent accuracy, savings, uptime or certification. Do not claim employer affiliation, endorsement or knowledge of employers' internal stacks.
- If browsing or execution is unavailable, say so and request the smallest useful evidence.

## Safety, security and resources

- Outputs are advisory only. Do not integrate with live industrial control networks, physical actuators, sensitive operational data or weapons-specific modelling.
- Never request passwords or tokens in chat or structured questions. Use local secret mechanisms such as an ignored `.env`, Docker secrets or an OS keychain. Sanitize logs, sample configurations, metadata and screenshots.
- No paid cloud deployments, unapproved system installations, or downloads and jobs beyond approved resource budgets. Do not launch subagents or mode handoffs unless the user approves them.
- Keep bulk datasets, caches, virtual environments, credentials, volumes and large binaries out of Git. Use small lawful fixtures. Public access to a dataset does not grant redistribution rights. Verify data, model, code and dependency licenses separately.
- Never `git add .`. Stage only the approved, inspected files. Never fabricate, backdate or rewrite commit history. A commit must represent a genuine, tested increment.

## Engineering baseline

- Quality checks, tests, documentation, security, governance and provenance start with the first relevant implementation. Dedicated stages consolidate and verify them; they do not postpone them.
- Prefer established libraries and simple baselines. Do not build a custom broker, a bespoke ML framework or an unnecessary feature-store service. Keep domain logic out of notebooks, DAGs, transport adapters and dashboards.
- Define delivery guarantees per source, processing step and sink combination, including external side effects. Checkpointing alone does not give exactly-once notifications or work orders.
- Run a focused check immediately after the first meaningful code edit, then broaden the implementation.

## Stage status vocabulary

Use these statuses: `not started`, `in progress`, `awaiting approval`, `blocked`, `complete` and `needs revalidation`. Mark a stage `complete` only after its approved acceptance criteria pass and the user accepts the outcome. Record deferred optional capabilities as deferred, never as implemented.

## Stage completion report (every stage)

Report the deliverables changed, checks run with actual results, decisions and ADRs, unverified items, the domain lesson, the proposed commit subject and file list (do not commit), and the next prompt to run manually.
