# IIoT Architecture Decision Records

Material decisions for the Industrial IoT project are recorded here as numbered ADRs (`NNNN-short-title.md`). Only record a decision as `accepted` when the user has explicitly selected it. Workflow preferences (granularity, learning style, Git policy) live in [project-state.md](../project-state.md) and are not ADRs.

## Index

| ADR | Title | Status | Stage | Affected stages |
| --- | --- | --- | --- | --- |
| [0001](0001-persona-and-mvp-scope.md) | Primary Persona and MVP Questions | accepted | 01 | 02–24 |
| [0002](0002-prediction-ambition.md) | Prediction Ambition | accepted | 01 | 02–24 |

Statuses: `proposed`, `accepted`, `rejected`, `superseded by NNNN`.

## Template

Copy into `NNNN-short-title.md`:

```markdown
# NNNN. <Decision title>

- Status: proposed | accepted | rejected | superseded by NNNN
- Date: YYYY-MM-DD
- Stage: iiot-NN-<name>
- Affected stages: NN, NN

## Question
<The decision that had to be made and why it matters.>

## Options considered
1. <Option A> — pros / cons / resource & learning implications
2. <Option B> — ...
3. <Custom / deferred>

## Recommendation
<Agent recommendation and reasoning, recorded as such.>

## User selection
<Exactly what the user chose, quoted or paraphrased faithfully. Leave empty while proposed.>

## Rationale and evidence
<Measurements, source links with verification dates, experiment results.>

## Consequences
<What this enables, constrains, or requires downstream; revalidation triggers.>
```
