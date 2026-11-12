# Change Proposal — Moto Touring Feasibility Refactor

## Why
Current route-generation systems can produce plausible itineraries without establishing whether a particular rider can complete the route given travel-time uncertainty, daylight, fuel, weather and road restrictions. The existing 13-section DESIGN.md needs to be translated into executable-style specifications and reviewable task gates.

## What Changes
- Formalize L2 pre-departure Generate–Verify–Repair with five agents.
- Ground ETA-dependent weather, fuel and road observations in timestamped tool results.
- Formalize P50 segment moving time and trip-level conformal P90 uncertainty.
- Apply hard rules R1 daylight, R2 fuel/hours, R3 weather windows, R4 road closures, R5 numeric provenance.
- Require explicit infeasibility output when attempts are exhausted or required data are missing.
- Define E1–E4, H1–H3 and an independent E2 oracle.

## Capabilities
- `planning-workflow`
- `pacing-and-privacy`
- `environment-grounding`
- `deterministic-verification`
- `research-evaluation`

## Scope
In: Proposal/spec-driven design, privacy requirements, pre-departure L2 system.  
Out: In-ride L3 MQTT/voice, accident prediction, demonstrated performance, application implementation.

## Success criteria
A reviewer can trace FR1–FR7, NFR1–NFR4 and rules R1–R5 to normative specs and test scenarios. All experimental outcomes remain explicitly prospective.
