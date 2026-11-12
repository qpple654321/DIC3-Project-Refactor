# DIC3 Proposal Refactor Handoff

**Status:** Documentation-only work, 2026-11-12.

## Purpose
Convert the original [Moto Touring Copilot DESIGN.md](https://github.com/yotsubamomo/moto-touring-copilot/blob/main/DESIGN.md) into spec-driven artifacts and make the academic framing auditable.

## Agreed source constraints
- L2: pre-departure multi-agent planning, tool-grounded calculations, verification and replanning.
- L3: continuous GPS/MQTT tracking and speech alerts are optional future extensions.
- Five agents: orchestrator, pacing, weather, road-and-supply, verification.
- Five rules: daylight, fuel and station hours, weather-window risks, road restrictions, numeric provenance.
- Trip time: predict time quantiles, apply held-out trip conformal calibration. Never treat speed P90 as time P90.
- E1–E4 are future experiments, not demonstrated findings.
- Keep raw GPX and secrets off public Git.

## Prepared handoff
A–F/5 cores/3 trade-offs/10 details; Introduction; Related Work; OpenSpec proposal/design/tasks/five specifications; rubric audit.

## Next steps
1. Install/initialize compatible OpenSpec version and preserve version/check logs.
2. Validate the change and correct schema differences, if any.
3. Compare revised prose and citations against canonical proposal PDF.
4. Add tests for five verifier rules, independently evaluate E2 oracle.
5. Implement only after specifications are reviewed. Report observed versus expected results separately.
