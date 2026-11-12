# Environment Grounding Specification

## ADDED Requirements

### Requirement: Arrival-time-aligned observations
The system SHALL request weather warnings, twilight, road controls and fuel-station hours for each location **at the predicted ETA window**, not simply at question time. All inputs SHALL contain source/time provenance.

#### Scenario: Station closed by arrival
- **GIVEN** an otherwise reachable station closes before conservative ETA
- **WHEN** evaluating refueling options
- **THEN** the station is not considered available.

#### Scenario: Warning time overlap
- **GIVEN** heavy-rain alerts intersect the segment ETA interval
- **WHEN** fetching environmental conditions
- **THEN** record the affected segment, risk and observation timestamp.

### Requirement: Data absence and staleness
The system MUST NOT invent CWA, TDX, station or daylight values when a critical external source is missing, stale or incompatible with the route/date.

#### Scenario: Missing road status
- **GIVEN** TDX road-status retrieval fails
- **WHEN** R4 requires it
- **THEN** the system reports the data gap and MUST NOT assert verified feasibility.
