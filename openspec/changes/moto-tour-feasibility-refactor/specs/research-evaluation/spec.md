# Research Evaluation Specification

## ADDED Requirements

### Requirement: Prospective results
All E1–E4 outcomes MUST be described as planned until an experiment is actually performed. Documentation MUST NOT claim improved accuracy, a pass rate, or zero hallucinations without observations.

#### Scenario: Proposal-only report
- **GIVEN** no trained/evaluated model
- **WHEN** writing Results
- **THEN** report Expected Outcomes and explicitly state that performance has not been measured.

### Requirement: Independent feasibility oracle
E2 SHALL score plans through a separate oracle using independently obtained/precomputed daylight, station-hour, closure and ride-time references; the runtime verification agent MUST NOT be its own ground-truth scorer.

#### Scenario: Ablation
- **GIVEN** identical test queries for LLM-only, LLM+grounding and full hybrid architecture
- **WHEN** evaluation finishes
- **THEN** report macro all-hard-constraints pass, per-rule violations, refusal rate, numerical consistency and provenance, judged independently.

### Requirement: Hypothesis-aligned metrics
H1 evaluates verification versus baseline. H2 evaluates the impact of authoritative grounding. H3 evaluates recovery under new disruptions. The protocol SHOULD also report MAE, MAPE, P90 trip coverage, daylight violation and fuel coverage.

#### Scenario: Disruption
- **GIVEN** a closure is introduced after candidate generation
- **WHEN** a static planner and a replanning configuration are evaluated
- **THEN** report successful recovery rate and unresolved or missing observations separately.

### Requirement: Four evaluations
E1 SHALL be pacing benchmarks with grouped/temporal splits. E2 SHALL be the independent-oracle ablation. E3 SHOULD backtest warnings and road disruptions where archived evidence exists. E4 SHOULD review 2–3 representative route cases.
