# 05 — DIC3 Assignment Audit

**Reviewed:** Source repository root, its DESIGN.md, proposal artifact listing, instructor sample repository and this refactoring package.

| Teacher requirement | Repository evidence | Remaining verification |
|---|---|---|
| Install OpenSpec; learn SDD | Setup/SDD guide | Run CLI on student's machine and capture version/validate evidence |
| A–F + 5 cores + 3 trade-offs + 10 details | docs/refactor/02-AF-5-3-10.md | Review instructor definitions of the 5/3/10 structure |
| Convert DESIGN.md into OpenSpec workflow | proposal, design, tasks, five capability specs | Confirm schema version with actual OpenSpec validation |
| Introduction refactor | docs/refactor/03-INTRODUCTION.md | Confirm citations and reconcile with original PDF |
| Related Work refactor | docs/refactor/04-RELATED-WORK.md | Verify paper titles, dates, numerical claims |

## Known consistency risks
1. **Scope:** DESIGN L2 is pre-departure planning. MQTT/voice in-ride tracking is L3 extension.
2. **Fatigue:** Pacing proxies are not yet validated physiological fatigue models.
3. **No safety guarantee:** Rules screen specified hazards, not accident probability.
4. **No measured outcomes:** E1–E4 are planned; do not claim improved MAE or zero hallucinations.
5. **Privacy:** raw GPX endpoints and private credentials must not be published.
6. **Oracle independence:** evaluate with a separate reference oracle, not the live verifier scoring itself.
7. **Literature:** reported success rates and novelty require primary-source verification.

## Submission evidence recommended
GitHub repository links to all five deliverables; OpenSpec CLI version/validation screenshot; verified bibliography; mapping from DESIGN requirements to capability specifications and acceptance tests; clear statement that this is a **proposal refactor** rather than completed implementation.
