---
description: "Generate interactive, phase-wise implementation prompts for an Industrial IoT and Predictive Maintenance portfolio platform."
agent: "agent"
---

# Generate the Industrial IoT Project Workflow

Act as a senior industrial data-platform architect, data engineer and technical mentor. Generate reusable VS Code GitHub Copilot stage prompts for the project below. Generate the workflow only: do not implement the application, run the stages, download data or create commits during this generator run.

This prompt is self-contained. It does not depend on the geospatial generator or any prior chat.

## Project Brief

- Working name: Industrial Asset Reliability & Predictive Maintenance Data Platform. The user may revise it during requirements discovery.
- Business problem: ingest equipment telemetry and maintenance records, detect abnormal operating conditions, diagnose supported fault states, and evaluate future failure prediction only where suitable longitudinal labels exist.
- Portfolio purpose: demonstrate senior data engineering and practical industrial-domain knowledge relevant to UAE employers such as EDGE, Sanad, Strata, AIQ, ADNOC, EGA and EMSTEEL. Do not imply endorsement, access to their data, confirmed vacancies or knowledge of their internal technology stacks.
- Domain: ordinary civilian industrial assets such as hydraulic pumps, compressors or cooling equipment. Choose one coherent equipment family for the MVP. No sensitive operational data, weapons-specific modelling, physical equipment control or autonomous maintenance actions.
- Candidate telemetry: temperature, vibration, pressure, RPM, operating hours and equipment status; related asset metadata, maintenance history and failure events. These are desired fields, not a claim that one source supplies them all.
- Candidate capabilities: condition monitoring, anomaly detection, supported fault diagnosis, maintenance inspection prioritisation, and optional failure-risk or remaining-useful-life benchmarks.
- Constraints: free data with verified reuse terms, open-source implementation, laptop execution, Docker Compose, Windows/WSL2 compatibility, no mandatory paid cloud service, Kubernetes, GPU or physical IoT hardware.
- Technology baseline carried forward for user confirmation: Python, Spark and Airflow. Candidate tools include Kafka for replayable event transport, optional MQTT/Mosquitto plus an explicit bridge, Delta/Parquet, PostgreSQL, scikit-learn, optional MLflow, Prometheus and Grafana. These are proposals, not already accepted architecture decisions. STAC is not needed for this project.
- Engineering requirements: medallion architecture, reusable modules, streaming and batch interfaces, explicit data contracts, point-in-time-correct features, quality gates, governance, security, lineage, monitoring, resource FinOps, tests, documentation and architecture diagrams.
- User: a senior data engineer with eight-plus years of experience learning industrial-domain concepts. Explain the domain and trade-offs, not elementary programming.
- Publication: public GitHub repository with lawful small examples, reproducible execution, truthful evaluation and genuinely incremental commit history.

## Candidate Data and Claim Boundaries

Treat the following as research starting points. Recheck authoritative documentation, current availability and permissions before adoption. Record source URLs and verification dates in the eventual source register.

1. UCI Condition Monitoring of Hydraulic Systems: https://archive.ics.uci.edu/dataset/447/condition+monitoring+of+hydraulic+systems. Real hydraulic test-rig measurements with pressure, temperature, vibration and other channels at different sampling rates, organised into 60-second cycles with component-condition labels. The source lists CC BY 4.0. It is not a fleet history or a documented time-to-failure trajectory. Do not manufacture operating hours, maintenance history, RPM or chronological degradation from row order.
2. UCI AI4I 2020: https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset. Synthetic observations with temperature, RPM, torque, tool wear and current failure labels; the source lists CC BY 4.0. Useful for a small classification example, not proof of advance warning. Failure-mode target columns cannot be model inputs when predicting the aggregate failure label.
3. NASA C-MAPSS via https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/. Simulated turbofan run-to-failure trajectories, potentially useful for a separate remaining-useful-life benchmark in operating cycles. Verify dataset-specific terms; do not describe it as real factory telemetry or convert cycles to hours without evidence.
4. Microsoft Azure predictive-maintenance samples: potentially useful synthetic telemetry/maintenance/failure examples, but establish original provenance and dataset permissions. A Kaggle mirror with an Unknown license is not an approved source. Do not assume the archived Azure PySpark tutorial and the five-file, 100-machine sample are the same dataset, or apply a repository code license automatically to external data.
5. A seeded, documented simulator may supply asset fleet context, operating states, correlated signals, degradation scenarios, maintenance events and transport faults. It demonstrates engineering behaviour under stated assumptions, not real-world predictive accuracy or a validated digital twin.

Keep measured experimental data, externally simulated benchmarks and custom synthetic fleets as separate provenance tracks. Do not merge unrelated sources into an apparently real asset history. Replaying a measured cycle under an assigned timestamp does not make that timestamp a historical observation time. An anomaly is not automatically an equipment fault; a fault diagnosis is not automatically a future failure prediction.

## First Interaction: Agree on the Workflow

Inspect current repository instructions, nearby workflow files and worktree state without making changes. Preserve existing work. This workspace may also contain a geospatial workflow; do not modify it or assume its decisions apply here.

Before generating files, ask a first batch of up to four questions with selectable options:

1. Granularity: the recommended 24 stages below; a compact pack with explicit responsibility mappings; or custom grouping.
2. Learning interaction: guided explanations and approval at material decisions (recommended); concise trade-offs with the same gates; or custom style.
3. Git policy: propose commits only (recommended); create a commit after each explicitly approved milestone; or no Git operations.
4. Repository arrangement, only if another project is present: keep namespaced workflow files here for now (recommended); use a separate user-specified workspace; or custom arrangement. Do not create another repository or application subfolder without approval.

Use the available structured question tool when supported. Otherwise show numbered questions with lettered options in chat and wait. Permit a custom answer. Never interpret silence as approval. Do not ask the full architecture interview now; hardware, asset type, source selection and topology belong to their stages.

After the user answers, generate the agreed prompt pack and supporting workflow files. Do not execute the first stage automatically.

## Files to Generate

- Stage prompts: `.github/prompts/iiot-01-*.prompt.md` through `.github/prompts/iiot-24-*.prompt.md`, as listed below unless the user approves a different grouping.
- `.github/prompts/README-iiot.md`: sequence, dependencies, expected outputs, invocation, decision checkpoints, resume instructions, selected Git policy, estimated effort labelled as estimates, and the difference between behavioral guidance and tool permissions.
- `.github/instructions/iiot-workflow.instructions.md`: concise shared interaction and safety contract. Use valid YAML frontmatter with a quoted description and `applyTo: ".github/prompts/iiot-*.prompt.md,docs/iiot/**"`. Every stage must explicitly read this contract, including when it edits application paths outside the glob. Expand patterns only after implementation paths are approved; do not impose industrial requirements on the geospatial project.
- `docs/iiot/project-state.md`: stage status, active stage, approved requirements/decisions, evidence, checks, blockers, unresolved questions and next action. Initialize all project stages as not started. Workflow preferences do not constitute architecture approval.
- `docs/iiot/decisions/README.md`: ADR template and empty decision index, without invented user selections.

Keep this generator and existing prompts intact. If a target exists, inspect it and ask before replacing or materially changing it. Prefer updating a compatible existing workflow over duplicating state, but never combine the industrial and geospatial project state silently.

Do not populate application code, infrastructure, datasets, model results, business requirements, CI or speculative architecture documents during generation. Those belong to approved stages.

Each stage must explicitly instruct the agent to read the shared contract, project state and relevant approved outputs. Link them with correct relative Markdown paths from `.github/prompts/`, for example `../instructions/iiot-workflow.instructions.md` and `../../docs/iiot/project-state.md`. A bare link is not a substitute for an instruction to read its contents. Expected future artifacts may be named as prerequisites, but must be identified as not yet created rather than presented as existing evidence.

## Shared Decision and Learning Contract

Encode these rules once in the shared contract; make every stage explicitly require them:

1. Start with prerequisite checks, current approved decisions, purpose and success criteria. Remain in the selected stage; never automatically run the next prompt.
2. Ask the user about every material business, domain, source, analytical, modelling, infrastructure, architecture, quality, governance, security, resource and release decision. Do not silently decide on their behalf. Routine coding details within an approved plan need no separate questions.
3. For each decision, present two to four realistic options, a recommended choice with reasons, pros/cons, implications for learning, resources and delivery, and a custom-answer option. Explain unfamiliar industrial concepts briefly. Do not manufacture alternatives just to fill a list.
4. Ask at most two to four related questions per interaction, through a structured question tool if available or lettered options in chat otherwise. Wait for the user's answer before implementing the affected choice. Do not hide independent decisions inside a single bundled option.
5. For evidence-dependent decisions, propose a bounded feasibility experiment, obtain approval for its downloads/edits/execution, report measurements and then ask for the durable choice. A temporary experiment does not approve an architecture.
6. After resolving decisions, propose a concise implementation plan and obtain explicit approval before substantive edits, installations or long-running jobs. Starting a stage authorizes read-only discovery, not every implementation choice.
7. Record material choices as numbered ADRs under `docs/iiot/decisions/`: question, alternatives, user's selection, rationale, evidence, consequences, status and affected stages. Distinguish proposed, accepted, rejected and superseded decisions. Never invent approvals.
8. Explain one useful domain lesson at each completed increment and show how it affects code or data. Do not turn the workflow into repetitive quizzes or explanations unrelated to the implementation.
9. Keep questions visible in chat, not only buried in documents. Stop at approval gates even if other agent instructions encourage autonomous continuation. Prompts guide behavior; they do not override higher-priority instructions or technically restrict tools.
10. Track changes to prior decisions and mark affected downstream outputs for revalidation. Resume from repository evidence and recorded state, not assumptions from an old chat.

## Stage Catalogue

Generate a tailored, actionable prompt for each row, not a generic template with substituted titles. Documentation outputs should live under `docs/iiot/` unless a shared root artifact is explicitly approved. Application/module paths remain architecture decisions.

| Prompt Filename | Responsibilities and Required Evidence |
| --- | --- |
| `iiot-01-business-requirements.prompt.md` | Identify users, asset-reliability workflow, industrial employer relevance, business questions, MVP/non-goals and measurable acceptance criteria. Separate platform reliability, anomaly detection, current diagnosis and future prediction. Define advisory-only inspection support. Produce requirements and a requirement-to-stage traceability table. |
| `iiot-02-industrial-domain-and-measurement.prompt.md` | Teach asset/component/sensor hierarchy, units/calibration, operating regimes, maintenance types, failure definitions, downtime, availability, MTBF/MTTR assumptions, sampling and signal bandwidth. Distinguish sensor faults from equipment faults and raw vibration waveforms from low-rate vibration indicators. Define measurement and label contracts without pretending simulated labels validate physical degradation. |
| `iiot-03-source-discovery-and-profiling.prompt.md` | Verify source provenance, original documentation, licenses, download availability, units, sampling rates, cycle/asset identities, time semantics, labels and missing fields. Propose bounded sample retrieval; profile real observations and record measured sizes/counts. Build a source register, field-coverage matrix and permitted-claim matrix. Do not equate public access with redistribution permission. |
| `iiot-04-equipment-scope-and-feasibility.prompt.md` | Ask about laptop CPU/RAM/free disk, Docker/WSL2, available time, network and retention budgets. Select one equipment family, approved data tracks and an initial replay workload. Propose a small replay/processing probe measuring runtime, memory, disk and label feasibility without requiring full infrastructure. Define go/no-go gates and achievable diagnosis versus forecasting scope. |
| `iiot-05-solution-and-data-architecture.prompt.md` | Compare transport/storage/compute/serving options and confirm Python/Spark/Airflow roles. Compare Kafka-only replay with optional MQTT plus explicit bridge; explain broker durability and sink-dependent delivery semantics. Select medallion formats, service profiles, offline operation, boundaries and resource envelopes. Produce ADRs and Mermaid context, component, data-flow, deployment and failure-path diagrams. Do not include STAC or unnecessary cloud dependencies. |
| `iiot-06-data-modelling-and-contracts.prompt.md` | Model sites, lines, assets, components, sensor definitions/calibration, telemetry, operating states, maintenance, replacements, failures, runs, features, scores, alerts and resource usage. Specify grains, IDs, units, timezone, event/ingestion/processing/availability times, sequence numbers and schema versions. Include bitemporal/correction needs, provenance tracks, component installation history, label availability and source-to-target mappings. Do not infer maintenance completion from an alert acknowledgement. |
| `iiot-07-governance-and-security-design.prompt.md` | Agree a threat model, local/public exposure, authentication/authorization, broker topic permissions, data classifications, source licensing, retention, audit/lineage, model approvals and secret lifecycle. Keep all use advisory and isolated from physical control. Produce executable control requirements and a lawful fixture/publication policy; do not promise safety certification or production OT compliance. |
| `iiot-08-repository-and-local-infrastructure.prompt.md` | Establish approved package boundaries, reusable entry points, configuration, dependency pins, tests, linting and Docker Compose. Separate persistent volumes and optional monitoring/model services, resource limits and health checks. Keep internal services non-public, credentials external and generated state ignored. Validate Windows/WSL2 paths, clean setup and non-destructive teardown. No infrastructure installation without approval. |
| `iiot-09-replay-and-synthetic-fleet.prompt.md` | Build reusable source replay and seeded simulator adapters as distinct provenance tracks. Preserve original cycle/sample identity, relative timing and resampling semantics; label replay timestamps. Simulate operating regimes, correlated signals, sensor drift/dropouts, failure/degradation scenarios and scheduled/reactive maintenance with documented assumptions. Keep hidden simulator state/labels out of scoring inputs. Do not clone cycles and claim independent real assets or inflate sample diversity. Test deterministic scenarios and bounded rates. |
| `iiot-10-ingestion-and-bronze.prompt.md` | Implement the approved transport and bronze acquisition with schema envelopes, source identity, event IDs, checksums/manifests, secure connections, backpressure and bounded retries. Cover broker persistence, restart, duplicate messages and malformed-event quarantine. If MQTT is chosen, explicitly implement/test the bridge and its replay limitations. Keep original source files separate from replay envelopes. Prove no silent loss under the agreed recovery test; do not assume end-to-end exactly-once. |
| `iiot-11-event-time-and-silver.prompt.md` | Implement unit normalization, schema evolution, calibration/state context, quality flags, deduplication, event-time windows, bounded state/watermarks and late-event policy. Define reconciliation for data beyond the streaming lateness bound. Handle different sensor rates, clock problems and corrections without silently forward-filling future information. Test replay, out-of-order data, missing/stuck sensors, restart and deterministic output. |
| `iiot-12-point-in-time-features-and-gold.prompt.md` | Implement reusable trailing-window features and maintenance/asset joins using only records available at scoring time. Define window closure, feature freshness, startup/idle handling, missingness and component replacement resets. Keep real-condition and synthetic future-risk feature contracts distinct. Prove offline/replay feature parity and absence of future information with targeted tests; avoid centered windows, whole-dataset scaling and retrospective maintenance leakage. |
| `iiot-13-baselines-and-model-evaluation.prompt.md` | Start with thresholds/statistical rules and simple supervised or anomaly models appropriate to labels. Predefine separate evaluation for current diagnosis, anomalies, future-horizon failure risk and optional RUL. Use experimental grouping where known for test-rig diagnosis; use time/asset-aware forecasting splits with embargo for overlapping feature/label windows. Fit preprocessing on training data only. Define horizon, censoring, rare failures and uncertainty; report event recall, false alerts per machine-day and lead time only where meaningful. Keep benchmarks/provenance tracks separate. |
| `iiot-14-scoring-and-alert-lifecycle.prompt.md` | Implement versioned scoring and an advisory inspection queue with evidence, thresholds, persistence, cooldown/hysteresis, deduplication and acknowledgement states. Define stale-feature/model handling, insufficient-data behavior and audit history. Separate alert creation, inspection result and actual maintenance completion. Demonstrate that replay does not duplicate actions and no command controls physical equipment. Do not interpret anomaly scores as calibrated failure probabilities. |
| `iiot-15-airflow-and-recovery.prompt.md` | Orchestrate tested package entry points for historical ingestion, backfills, feature recomputation, approved training/evaluation and publication. Keep the continuous streaming runtime lifecycle explicit; do not schedule one DAG per sensor event or hide a daemon in an ordinary retrying task. Define checkpoint ownership, retries, timeouts, concurrent-run protection and safe recovery. Demonstrate interrupted processing and replay without duplicate gold records or alerts. |
| `iiot-16-quality-and-validation-gates.prompt.md` | Consolidate executable bronze/silver/gold and model-input quality contracts already introduced earlier. Cover IDs, units, ranges, clock drift, stuck sensors, missing periods, rate changes, source drift, label/feature availability and provenance separation. Define warn/fail/quarantine behavior and freshness-aware promotion. Inject bad data to prove it cannot silently produce a healthy equipment status or valid prediction. |
| `iiot-17-serving-and-maintenance-interface.prompt.md` | Serve approved asset health, telemetry trends, data-quality status, provenance, scores and inspection queue through a minimal API/dashboard. Include units, observation times, freshness, operating regime, model version and uncertainty. Distinguish measured, replayed and synthetic data visibly. Implement useful filtering, evidence inspection and acknowledgement with audit records. Test API contracts and nonblank desktop/mobile UI without promising certified maintenance guidance. |
| `iiot-18-observability-and-runbooks.prompt.md` | Add correlated logs/metrics for broker lag, throughput, freshness, late/dropped/quarantined events, checkpoints, quality, alert volume, API health, memory and disk. Define actionable local alerts and runbooks; test an alert and recovery. Avoid secrets and high-cardinality metric labels. Separate event-time lag from accelerated-replay wall time and acknowledge laptop downtime; use optional monitoring profiles. |
| `iiot-19-model-lifecycle-and-drift.prompt.md` | Implement agreed model/feature/data versioning, reproducible training, registry/artifact integrity and controlled promotion/rollback; consider MLflow only if justified. Monitor feature and prediction drift separately from model accuracy. Evaluate performance only when delayed labels mature, and account for maintenance interventions altering outcomes. Keep retraining and deployment human-approved; demonstrate rollback and never treat synthetic drift tests as evidence of real-world adaptation. |
| `iiot-20-security-and-governance-verification.prompt.md` | Verify earlier controls: broker/API permissions, local bind addresses, credentials and redaction, dependency/container/secret scanning, license notices, audit/lineage, model provenance and safe retention/deletion. Inspect public fixtures, screenshots and history for secrets or restricted data. Document residual risk and prove at least one denied operation. Dedicated verification does not excuse postponing baseline controls until now. |
| `iiot-21-performance-and-resource-finops.prompt.md` | Benchmark approved replay and backfill workloads with actual event rate, latency percentiles, CPU time, peak memory, storage growth, broker retention and cost per meaningful unit. Compare selected batching, partitioning and caching choices, including a simpler baseline where appropriate. Separate wall time from CPU time, synthetic load from physical fidelity, and hypothetical cloud prices from measured local costs. Add budget guardrails and non-destructive cleanup. |
| `iiot-22-end-to-end-and-reproducibility.prompt.md` | Validate requirement traceability, clean-environment tiny demo, an approved real experimental sample, separate synthetic scenarios, late events, restart, backfill, schema changes, quarantine, point-in-time joins and alert deduplication. Use network-free lawful fixtures in ordinary CI and opted-in source integration tests. Test train/inference compatibility and delayed labels where implemented. Record commands, hardware, actual results and unverified criteria without invented accuracy or savings. |
| `iiot-23-documentation-and-portfolio-story.prompt.md` | Polish quickstart, developer guide, data dictionary, source register, domain glossary, measurement/model cards, ADR index, runbooks, benchmarks and Mermaid diagrams against actual code. Add small lawful screenshots and a reproducible demo narrative. Explain senior engineering trade-offs, limitations, synthetic-versus-measured results and transferable industrial knowledge without claiming employer affiliation, production deployment or quantified downtime savings without evidence. |
| `iiot-24-github-release-and-handover.prompt.md` | Review public readiness for secrets, source/code/dependency licensing, large files, CI, reproducibility and documentation. Ask for code-license approval separately from data terms. Propose repository metadata, truthful release notes and portfolio summary. Review genuine incremental commits without backdating or rewriting them. Repository creation, pushes, tags and releases require explicit authorization; provide manual steps if auth/tooling is unavailable. Deliver a requirement/evidence checklist and prioritized backlog. |

## Required Structure of Every Stage Prompt

Use valid YAML frontmatter with a meaningful quoted `description` and `agent: "agent"`. Do not pin a model, invent tool identifiers or require an unavailable custom agent. Make each stage usable in a fresh Agent chat.

Each stage must contain:

1. Objective, scope and non-goals.
2. Required context, predecessor approvals and expected input artifacts. Identify missing prerequisites and stop rather than inventing them. For optional capabilities, state the condition that makes them applicable and how approved deferral is recorded.
3. A short industrial-domain learning objective tied to the work.
4. Stage-specific decision questions with realistic options and a reasoned recommendation, not assumed user choices.
5. Research or a bounded experiment where evidence is needed.
6. An explicit wait-for-answer rule and an implementation-plan approval gate.
7. Concrete work steps using existing reusable modules and repository conventions.
8. Named deliverables, owners and interface/contract changes. Prefer updating existing artifacts over duplicating frameworks or documentation.
9. Focused executable acceptance checks for code and clear review criteria for documents. Explain what would falsify success. Run a focused check immediately after the first meaningful code edit before broadening the implementation.
10. Blocker handling, tool/network limitations and explicit unverified requirements.
11. State, ADR and requirement-traceability updates, plus a concise completion report.
12. A truthful proposed commit subject and the next prompt to run. Never automatically execute that next stage.

Keep shared rules in the contract, but include concrete decision prompts and checks specific to each stage. Do not simply copy this generator into every file.

## Cross-Stage Engineering and Publication Rules

- Quality, documentation, security, governance, tests and provenance begin with the first relevant implementation. Dedicated stages consolidate and verify them, not postpone them.
- Prefer established libraries and simple baselines; no custom broker, bespoke ML framework or unnecessary feature-store service. Keep domain logic outside notebooks, DAGs, transport adapters and dashboards.
- Distinguish source-observation time, cycle-relative time, assigned replay time, ingestion time and availability time. Never interpret arbitrary cycle order as true continuous machine history.
- Replay tests demonstrate transport/compute behavior. Synthetic scenarios demonstrate behavior under assumptions. Neither proves equipment safety, transferable physical models or industrial predictive accuracy.
- Define delivery guarantees per source, processing and sink combination, including external side effects. Checkpointing alone does not establish exactly-once notifications or work orders.
- Keep high-rate waveforms separate from low-rate summaries. Respect sampling limits; do not claim low-frequency vibration readings support high-frequency bearing diagnostics.
- Forecasting requires defensible future labels, horizon, observation availability, censoring and leakage controls. Insufficient future follow-up is not automatically a negative label. Failure events cannot be deduced from retrospective maintenance records without documenting the assumptions.
- Keep test-rig diagnosis evaluation separate from continuous-history forecasting evaluation. Do not multiply independent sample counts by replaying or cloning the same cycles.
- Use only public, lawful civilian examples and advisory outputs. No integration with live industrial control networks or physical actuation. Do not invent safety/compliance certification or employer-specific operating knowledge.
- Research authoritative sources and distinguish verified facts, measurements, assumptions and proposed targets. If browsing or execution is unavailable, state the limitation and request the smallest useful evidence.
- Use portable configuration, compatible pinned dependencies and small reproducible fixtures. Keep bulk datasets, trained binaries where impractical, caches, virtual environments, credentials and volumes out of Git.
- Never request passwords/tokens through chat or structured questions. Use local secret mechanisms and retain user tool approvals. Sanitize logs, sample configs, metadata and screenshots.
- A public code repository does not require public redistribution of its training data. Verify data/model/dependency licenses independently and preserve attribution.
- Respect the approved Git policy. Do not initialize Git, create branches, commit, push or publish without authorization for that operation. Inspect changes and stage only approved files, never blanket-stage with `git add .`.
- A meaningful commit represents a genuine, tested logical increment. Never fabricate history, backdate commits, rewrite user commits or split an already-finished project merely to simulate incremental development. Prefer truthful subjects such as `feat(ingestion): persist telemetry with replay-safe event keys` when actually implemented and tested.
- No paid deployments, unapproved system installations or downloads/jobs beyond approved resource budgets. No automatic subagents or mode handoffs; propose them and obtain approval first if needed.
- Track states honestly: not started, in progress, blocked, awaiting approval, complete. Mark complete only after applicable approved acceptance criteria pass and the user accepts the stage outcome. Record deferred optional capabilities rather than claiming they were implemented.

## Generator Validation and Handover

Before reporting success:

1. Validate frontmatter, unique numbered filenames, relative links to created support files, and correct naming of future prerequisites.
2. Map every catalogue responsibility to a generated stage, even when the user selected compact grouping. Check coverage of telemetry, maintenance context, simulation/replay, event-time processing, data modelling, analytical validity, operations, governance/security and public delivery.
3. Check every stage requires contextual reads, option-based decisions, waiting for answers, approval before implementation, concrete outputs, falsifiable acceptance checks and a manual handoff.
4. Verify no industrial decision has been falsely accepted and no geospatial file was overwritten or industrial rule applied globally to the other project.
5. Confirm no application code, installation, dataset download, Git mutation or remote publication occurred during generation.
6. Summarize generated files, selected interaction/Git policies and how to launch the first stage through the prompt picker or `Chat: Run Prompt...`. Stop and let the user launch it.

Explain that prompts guide behavior but are not an enforcement mechanism. The user should retain VS Code tool approvals and review diffs, especially for shell, Git and network operations.