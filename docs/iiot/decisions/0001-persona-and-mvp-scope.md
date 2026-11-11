# 0001. Primary Persona and MVP Questions

- Status: accepted
- Date: 2026-11-11
- Stage: iiot-01-business-requirements
- Affected stages: 02–24

## Question

Which user should anchor the MVP, and which business questions should it answer? This shapes the evidence and workflow required without selecting sources, equipment or technology.

## Options considered

1. **Reliability engineer; four selected questions** — Pros: connects condition evidence, diagnosis and inspection rationale; strong domain learning. Cons: needs varied evidence and an explicit "unsupported" outcome. Resource/delivery effect: moderate analytical and UI scope, still advisory.
2. **Maintenance planner** — Pros: directly supports queue prioritisation and maintenance workflow learning. Cons: risks implying work-order integration or requiring planning context. Resource/delivery effect: potentially broader workflow/integration scope.
3. **Plant data/platform engineer** — Pros: focuses on telemetry health and pipeline learning; comparatively narrower analytical scope. Cons: offers less direct maintenance decision support. Resource/delivery effect: simpler MVP, but less alignment with asset-reliability questions.
4. **Custom persona or question set** — Pros: adapts to a different portfolio goal. Cons: scope and evidence needs are unknown until specified. Resource/delivery effect: cannot be estimated in advance.

## Recommendation

Anchor the MVP on a reliability engineer and include a small set of complementary questions covering condition, component evidence, inspection priority and telemetry trust. This connects the reliability workflow while keeping outputs advisory.

## User selection

The user selected **reliability engineer** and all four offered questions:

- Which assets are in an abnormal operating condition now?
- Which component condition is currently degraded?
- Which assets should be inspected first, and why?
- Is telemetry trustworthy and fresh?

## Rationale and evidence

Selection was made by the user in Stage 01. No additional rationale or source evidence was supplied. The recommendation above is the agent's, not attributed to the user.

## Consequences

Later stages must support evidence/trend review and inspection prioritisation, while reporting when a question cannot be answered from available data. The question set does not establish that any source supports component diagnosis or prediction.
