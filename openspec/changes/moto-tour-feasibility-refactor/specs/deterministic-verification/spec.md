# Deterministic Verification Specification

## ADDED Requirements

### Requirement: Five deterministic checks
The system SHALL evaluate applicable physical and numerical constraints independently of LLM self-judgment, returning both pass/fail and explicit rule identifiers.

- **R1 — Daylight:** departure time + calibrated trip P90 SHALL NOT exceed the earlier of civil twilight end or rider-defined deadline.
- **R2 — Refueling:** consecutive feasible fuel stops SHALL be within reserve-adjusted vehicle range and selected stations MUST be open at conservative ETA.
- **R3 — Weather:** heavy-rain-or-higher warning overlapping a segment ETA window is high risk; the defined policy SHALL reject a high-risk segment in the final daylight-critical portion.
- **R4 — Road restrictions:** route closures and applicable access restrictions SHALL invalidate affected candidates.
- **R5 — Numerical traceability:** every quantitative value in verified output SHALL map to a source/tool output and computation.

#### Scenario: Daylight cutoff
- **GIVEN** a rider cutoff of 18:00 and predicted conservative completion at 18:30
- **WHEN** the verifier evaluates R1
- **THEN** it rejects the candidate and identifies the violated cutoff.

#### Scenario: Fuel insufficiency or closed station
- **GIVEN** a refueling gap exceeds reserve-adjusted range, or a selected station closes before P90 arrival
- **WHEN** R2 is evaluated
- **THEN** the verifier rejects the route and reports that gap or station.

#### Scenario: Heavy rain in critical final segment
- **GIVEN** heavy-rain warning overlap for a segment in the final daylight-critical portion
- **WHEN** R3 is evaluated
- **THEN** the verifier reports high risk and rejects the candidate under this rule policy.

#### Scenario: Road closure
- **GIVEN** TDX data indicating an applicable closure on the planned road
- **WHEN** R4 is evaluated
- **THEN** the itinerary fails with source timestamp.

#### Scenario: Unsupported numeric claim
- **GIVEN** the text contains a time or distance not justified by a tool result
- **WHEN** R5 is evaluated
- **THEN** the unverified quantitative claim MUST NOT be released as validated.
