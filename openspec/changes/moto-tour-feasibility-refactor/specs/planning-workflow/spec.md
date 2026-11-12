# Planning Workflow Specification

## ADDED Requirements

### Requirement: Complete rider constraints
The system SHALL request trip date, origin/destination or waypoints, latest return time, vehicle/range information, and any essential rider limits before making numerical feasibility claims.

#### Scenario: Missing deadline
- **GIVEN** no rider return deadline
- **WHEN** a query requests an itinerary
- **THEN** the orchestrator requests that missing input rather than inventing a deadline.

### Requirement: Bounded Generate–Verify–Repair
The planner SHALL generate candidate departure times/routes, acquire required tool data, verify R1–R5 and attempt bounded corrections when violated. After the configured attempt budget it SHALL either return a fully checked candidate or a clearly labeled infeasibility report.

#### Scenario: Failed candidate
- **GIVEN** a candidate violates R1
- **WHEN** verification returns that violation
- **THEN** the planner adjusts a permitted variable (departure time, route, stops) and re-verifies.

#### Scenario: Exhaustion
- **GIVEN** all candidates fail or required data remain unknown
- **WHEN** attempt limit is reached
- **THEN** the system reports violations, uncertainty and possible alternatives; it MUST NOT label the plan safe.

### Requirement: Traceable rider output
The final result SHALL include departure recommendation, route, conservative ETA, refueling needs, identified hazards and cited numerical sources.

#### Scenario: Passing itinerary
- **GIVEN** a fully verified candidate
- **WHEN** rendering the plan
- **THEN** every reported computed number identifies a corresponding tool output.
