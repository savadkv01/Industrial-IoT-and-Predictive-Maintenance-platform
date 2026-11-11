# 0002. Prediction Ambition

- Status: accepted
- Date: 2026-11-11
- Stage: iiot-01-business-requirements
- Affected stages: 02–24

## Question

Should future failure forecasting be a core MVP goal, an optional benchmark, or outside the project scope? Forecasting needs defensible future labels, a stated horizon, observation availability, censoring handling and leakage controls.

## Options considered

1. **Monitoring and diagnosis in the MVP; forecasting as an optional benchmark** — Pros: preserves a useful MVP if longitudinal labels are scarce and supports learning about evaluation limits. Cons: forecasting may remain unimplemented. Resource/delivery effect: bounded MVP; forecast work is conditional. Recommended.
2. **Forecasting as a core MVP goal, subject to source evidence** — Pros: foregrounds predictive modelling and its evidence requirements. Cons: insufficient labels or follow-up could block delivery and create claim risk. Resource/delivery effect: higher profiling and evaluation effort; schedule depends on source feasibility.
3. **No forecasting** — Pros: narrows validation and delivery work. Cons: removes an optional learning/evaluation path. Resource/delivery effect: smallest analytical scope.
4. **Custom ambition** — Pros: permits a different boundary. Cons: implications are unknown until specified. Resource/delivery effect: depends on requested scope.

## Recommendation

Keep monitoring and diagnosis in the MVP and make forecasting an optional benchmark conditional on source evidence. This balances learning and delivery effort with the uncertainty around longitudinal labels.

## User selection

The user selected **monitoring and diagnosis in the MVP, with forecasting as an optional benchmark**.

## Rationale and evidence

Selection was made by the user in Stage 01. No dataset or label evidence has yet been evaluated. The recommendation above is the agent's, not attributed to the user.

## Consequences

Stages 02–04 must define and assess label, time and follow-up needs before any forecast is attempted. If evidence is insufficient, forecasting remains deferred; anomaly scores must not be presented as failure probabilities.
