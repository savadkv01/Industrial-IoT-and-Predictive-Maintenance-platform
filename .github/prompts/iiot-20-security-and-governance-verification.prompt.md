---
description: "IIoT Stage 20: verify the implemented security and governance controls (permissions, bind addresses, secrets, scans, licences, lineage, retention) and document residual risk with at least one proven denied operation."
agent: "agent"
---

# IIoT Stage 20: Security and Governance Verification

## Read first
Read these files in full and follow them:
- [Shared workflow contract](../instructions/iiot-workflow.instructions.md)
- [Project state](../../docs/iiot/project-state.md)
- [Decision index](../../docs/iiot/decisions/README.md)
- `docs/iiot/governance/threat-model.md`
- `docs/iiot/governance/control-requirements.md`
- `docs/iiot/governance/publication-policy.md`
- `docs/iiot/sources/source-register.md`

## 1. Objective, scope and non-goals
- **Objective:** verify, with evidence, that every control requirement from Stage 07 is implemented, and record the residual risk.
- **Non-goals:**
  - building baseline controls for the first time here. A missing baseline control is a finding that belongs to its owning stage.
  - penetration testing beyond the local scope;
  - compliance certification.

## 2. Prerequisites and inputs
- The implemented services from Stages 08–19.

## 3. Domain learning objective
Explain how a compromised or spoofed telemetry source can produce false maintenance priorities. Show why topic ACLs and provenance metadata limit that risk.

## 4. Decisions to put to the user
1. **Scanners:**
   - (a) open-source tools: `pip-audit` for dependencies, Trivy or Grype for container images, gitleaks or detect-secrets for secrets, and a licence report tool (recommended);
   - (b) a subset;
   - (c) custom.
   Ask for approval of each installation.
2. **Severity policy:** decide which findings block the release and which can be accepted with a documented rationale.
3. **Git history scan:** if Git exists, ask whether to scan the full history. Never rewrite history without explicit user instruction.
4. **Residual-risk acceptance:** the user explicitly accepts or rejects each residual risk.

## 5. Research or experiment
Run the approved scanners. Run the control tests:
- unauthenticated broker publish;
- unauthorised API state change;
- port binding inspection;
- retention job dry-run.

## 6. Gates
1. Wait for the scanner and policy decisions.
2. Get approval before installing tools or scanning history.
3. Get approval of the remediation plan before fixing findings.

## 7. Work steps
1. Build a control verification matrix. For each control, record its ID, the test or command, the result, the evidence path and the status.
2. Run the scanners and triage the findings.
3. Inspect the items that will be published:
   - fixtures;
   - screenshots;
   - sample configs;
   - logs;
   - notebooks, if present;
   - Git history, if approved.
   Look for secrets, restricted data and attribution gaps.
4. Verify the licence notices and attribution for data, dependencies and models.
5. Verify the audit and lineage completeness for one end-to-end run.
6. Verify that the retention and deletion jobs are safe: they need a dry-run, and destructive deletion requires confirmation.
7. Document the residual risks and the user's acceptance decisions.

## 8. Deliverables
- `docs/iiot/governance/verification-report.md`
- Scanner configs and scripts.
- Remediation changes, if they were approved.
- ADRs for risk acceptance.

## 9. Acceptance checks
- At least one denied operation is proven with captured output.
- No secrets are found in tracked files, or each finding is remediated.
- Every external data source has attribution.
- No service is bound publicly without approval.
- **Falsified if:** any control is marked verified without evidence, or high-severity findings are left untriaged.

## 10. Blockers and unverified items
If scanners cannot be installed, list the manual checks performed and mark the rest `unverified`.

## 11. State, ADR and traceability updates
Update the state, the ADRs and the traceability table. Give the domain lesson. Finish with the completion report.

## 12. Handoff
- Proposed commit subject: `chore(security): verify controls, add scanning and document residual risk`. Do not commit unless the user asks.
- Next prompt: `iiot-21-performance-and-resource-finops`.
