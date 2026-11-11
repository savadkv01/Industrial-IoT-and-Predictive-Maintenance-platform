---
description: "IIoT Stage 21: benchmark approved replay and backfill workloads (rate, latency percentiles, CPU, memory, storage, retention, cost per unit), compare tuning choices against simpler baselines, and add budget guardrails."
agent: "agent"
---

# IIoT Stage 21: Performance and Resource FinOps

## Read first
Read in full and follow:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/feasibility.md`, which holds the budgets and the probe baseline
- `docs/iiot/architecture/architecture.md`
- `docs/iiot/operations/observability.md`

## 1. Objective, scope and non-goals
- **Objective:** measure the actual resource behaviour against the approved budgets.
- **Comparisons:** compare the selected tuning choices with a simpler baseline.
- **Guardrails:** add budget guardrails and non-destructive cleanup.
- **Non-goals:**
  - cloud deployment;
  - presenting hypothetical cloud prices as measured costs;
  - presenting synthetic load as physical fidelity.

## 2. Prerequisites and inputs
- A working end-to-end pipeline.
- The observability metrics from Stage 18.

## 3. Domain learning objective
Explain how sensor counts, sampling rates and retention multiply into storage growth. Show why industrial platforms downsample and tier their history.

## 4. Decisions to put to the user
1. **Workloads:** select from these and confirm their sizes:
   - steady replay at 1× and N× the target rate;
   - historical backfill of X days;
   - feature recomputation;
   - training.
2. **Comparisons:** choose 1–3 to run:
   - partitioning scheme;
   - micro-batch size or trigger interval;
   - Spark versus a lightweight engine for the same job;
   - file compaction;
   - caching.
3. **Cost unit:** for example, CPU-seconds and bytes per million events, or per asset-day. Ask whether to add an *illustrative* cloud price estimate, clearly labelled hypothetical, with its source and date.
4. **Guardrails:** disk-usage thresholds, broker retention caps, container memory limits, and a cleanup policy that is dry-run by default.

## 5. Bounded experiment (requires approval)
1. State the runtime, disk and RAM expected for each benchmark.
2. Run each benchmark at least three times on a warm stack.
3. Record:
   - hardware;
   - wall time and CPU time;
   - p50, p95 and p99 event-time-to-alert latency;
   - throughput;
   - peak memory;
   - storage growth.

## 6. Gates
Get approval of the benchmark plan before running long jobs. Get approval of the tuning changes before applying them.

## 7. Work steps
1. Write a benchmark harness that reuses the CLIs and emits machine-readable results.
2. Run the baselines and comparisons, and report the medians with their spread.
3. Compute the cost per unit from the measurements.
4. Implement the guardrails: disk-check commands or alerts, and cleanup with a dry-run plus an explicit confirmation.
5. Update the resource envelopes in the architecture document when the measurements differ from the estimates.

## 8. Deliverables
- The benchmark harness and its results files.
- `docs/iiot/performance/benchmarks.md`
- The guardrail scripts.
- ADRs for any tuning changes.

## 9. Acceptance checks
- The benchmarks are reproducible with documented commands.
- The results include the hardware and the variance.
- Wall time and CPU time are reported separately.
- The guardrail dry-run lists its targets without deleting anything.
- **Falsified if:**
  - a single run is reported as representative;
  - estimates are presented as measurements;
  - the cleanup deletes data without confirmation.

## 10. Blockers and unverified items
Document any thermal or background-load noise on the laptop. Mark cloud figures as hypothetical.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `perf(iiot): benchmark workloads and add resource guardrails`. Do not commit unless the user asks.
- Next prompt: `iiot-22-end-to-end-and-reproducibility`.
