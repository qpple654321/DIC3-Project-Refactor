# Tasks — DIC3 Moto Tour Feasibility Refactor

## Documentation and specification
- [x] Extract FR1–FR8, NFR1–NFR4, R1–R5, E1–E4 from source DESIGN.md.
- [x] Write A–F, five domain principles, three trade-offs and ten technical details.
- [x] Draft refactored Introduction and Related Work.
- [x] Draft OpenSpec change proposal, system design and five capability specifications.
- [ ] Validate the OpenSpec change with the actually installed CLI and keep proof.
- [ ] Check bibliography against original publications and update canonical proposal.

## Implementation (not yet performed)
- [ ] Implement private GPX ingestion/anonymized derivative exports.
- [ ] Add geometry, DTM and terrain/road segment features.
- [ ] Implement moving-time quantiles and trip-level conformal calibration.
- [ ] Integrate CWA, TDX, station data and timestamped provenance.
- [ ] Implement five specialized agents and bounded replan controller.
- [ ] Implement R1–R5 as deterministic testable functions.
- [ ] Implement clear infeasibility and missing-data outputs.
- [ ] Produce UI and downloadable source-grounded itinerary.

## Verification and experiments (not yet performed)
- [ ] Unit test each hard rule and API outage behavior.
- [ ] Evaluate pacing with GroupKFold and temporal holdout (E1).
- [ ] Build independent E2 oracle; run controlled ablations.
- [ ] Backtest historical available warnings/closures (E3).
- [ ] Run representative GPX case studies (E4).
- [ ] Report H1–H3 and limitations; distinguish predictions from observations.
