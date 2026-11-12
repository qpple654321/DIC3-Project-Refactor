# Design — Moto Touring Copilot

## Architecture
The LLM orchestrator interprets requirements, delegates tools, composes candidates and explains verified outcomes. A deterministic Python analytics layer owns calculations and rule decisions; the LLM cannot self-certify itinerary feasibility.

**Five agents:** (1) Orchestrator, (2) Pacing, (3) Weather, (4) Road & Supply, (5) Verification.

## End-to-end workflow
```text
Rider input → UNDERSTAND / clarify missing constraints
  → BUILD_ROUTE and SEGMENT_FEATURES
  → CANDIDATE_DEPARTURES
  → PACE_AGENT (P50 + trip-calibrated P90/ETAs)
  → WEATHER_AGENT (ETA-matched CWA warnings/twilight)
  → ROAD_SUPPLY_AGENT (TDX closures, station hours/range)
  → ORCHESTRATOR_COMPOSE
  → VERIFY R1–R5
  ├─ pass → choose maximal feasible slack → provenance-linked result
  └─ fail → violation feedback → REPLAN within bounded attempts
             └─ exhausted → explicit infeasibility report
```

## Time model
Train segment **log moving time** against route features; avoid speed-quantile inversion. Estimate P50 via quantile regression; aggregate with rest/break allowances and use held-out-trip conformal calibration for the total-trip P90. Do not directly sum each segment's P90 and assume calibrated whole-trip coverage.

## Data
Private GPX (trim home/work endpoints in public derivatives), DTM and road geometry, CWA, TDX, fuel locations/hours and optional mapping route baseline. Store provenance timestamps and model versions. On source failure, do not invent substitute observations.

## Three key trade-offs
1. P90 robustness versus route feasibility: calibrate whole-trip coverage and report rejection.
2. Data freshness versus availability: explicit unknown/fail-closed on missing critical sources.
3. Rider-specific prediction versus location privacy: private GPX and minimized derived exports.

## Evaluation
E1: per-trip grouped/temporal predictive benchmarks. E2: ablate LLM-only, LLM+grounding and complete system, judged by **independent** oracle. E3: retrospective hazard/closure evidence where archives permit. E4: 2–3 case studies. None are complete at Proposal stage.
