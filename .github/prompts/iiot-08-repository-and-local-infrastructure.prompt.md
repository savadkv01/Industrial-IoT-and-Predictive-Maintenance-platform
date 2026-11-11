---
description: "IIoT Stage 08: set up the approved package layout, configuration, pinned dependencies, tests and linting, and a Docker Compose stack with profiles, limits, health checks and safe teardown on Windows and WSL2."
agent: "agent"
---

# IIoT Stage 08: Repository and Local Infrastructure

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/feasibility.md`, for resource budgets
- `docs/iiot/architecture/architecture.md`
- `docs/iiot/data-model/data-model.md`
- `docs/iiot/governance/control-requirements.md`

Stop if Stages 05–07 are not accepted.

## 1. Objective, scope and non-goals
- **Objective:** build a reproducible skeleton that later stages extend. It includes:
  - the package boundaries;
  - reusable entry points (CLI);
  - typed configuration;
  - pinned dependencies;
  - test and lint tooling;
  - a Compose stack implementing the approved services.
- **Non-goals:** domain logic, pipelines and models. Do not install system software without approval. CI workflows wait until Stage 22, unless the user wants a minimal lint and test CI now.

## 2. Prerequisites and inputs
- Component ADRs, resource envelopes and security controls owned by this stage: bind addresses, the `.env` pattern and non-root containers.
- The workspace is not a Git repository. Do **not** run `git init` unless the user explicitly asks.

## 3. Domain learning objective
Explain why industrial data platforms keep the edge or ingestion tier, the analytics tier and the presentation tier separate in deployment. Show how Compose profiles mirror that separation.

## 4. Decisions to put to the user
1. **Package layout:**
   - (a) a single `src/` Python package with subpackages for contracts, ingestion, processing, features, models, scoring, serving and common code (recommended);
   - (b) multiple installable packages;
   - (c) custom.
   Also agree the top-level folder names for `infra/`, `dags/`, `tests/`, `configs/` and `data/` (ignored).
2. **Python tooling:**
   - (a) `uv` or `pip-tools` with a lock file, plus ruff, mypy and pytest (recommended);
   - (b) Poetry;
   - (c) conda;
   - (d) custom.
   Choose the Python version, which must be compatible with the chosen Spark and Airflow versions.
3. **Compose profiles:**
   - `core`: broker, storage, Postgres;
   - `orchestration`: Airflow;
   - `ml`: MLflow;
   - `monitoring`: Prometheus and Grafana.
   Confirm the profiles, the per-service memory and CPU limits, and the named volumes.
4. **Initialise Git:** ask whether the user wants to run `git init`, and whether they will run it themselves. Respect the user-initiated Git policy.

## 5. Research or experiment
- Verify that the pinned image tags and package versions exist and are compatible, for example Airflow's constraints file and Spark/Java compatibility.
- Propose a bounded `docker compose config` and `up` health probe, and get approval before pulling images. State the download size if it is known.

## 6. Gates
Wait for the decisions. Present the file tree and plan, and get approval before creating files or pulling images.

## 7. Work steps
1. Create the package skeleton with a working CLI entry point and a typed config loader (env plus file, no secrets in defaults).
2. Add `.gitignore` entries for data, volumes, `.env`, caches and model artifacts. Add `.env.example` with placeholders only.
3. Add lint, type-check and test configuration, plus one smoke test. Run it immediately.
4. Add a Compose file that:
   - uses pinned images and non-root users where supported;
   - binds services to `127.0.0.1` or internal networks;
   - sets health checks and resource limits;
   - uses separate persistent volumes and profiles.
5. Add Make, just or PowerShell task scripts that work on both Windows and WSL2. Document the path caveats.
6. Add a teardown command that stops containers without deleting volumes by default, plus a separately confirmed destructive purge.
7. Write the developer guide.

## 8. Deliverables
- Package skeleton
- Dependency and lock files
- Tool configuration
- Compose file
- Task scripts
- `docs/iiot/dev-guide.md`
- ADRs for the layout and tooling

## 9. Acceptance checks
- The lint, type-check and smoke tests pass.
- `docker compose config` validates.
- With approval, `docker compose --profile core up` reaches a healthy state, and the measured RAM fits the budget.
- Teardown keeps the volumes.
- Run the setup from a clean clone or copy.
- **Falsified if:**
  - any port binds to `0.0.0.0` without approval;
  - secrets appear in tracked files;
  - the default teardown deletes data.

## 10. Blockers and unverified items
If Docker is unavailable, deliver the Python skeleton, mark the Compose checks `unverified`, and record a blocker.

## 11. State, ADR and traceability updates
Update the project state with the commands and results, and record the ADRs. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `build(iiot): scaffold package, tooling and local compose stack`. Do not commit unless the user asks.
- Next prompt: `iiot-09-replay-and-synthetic-fleet`.
