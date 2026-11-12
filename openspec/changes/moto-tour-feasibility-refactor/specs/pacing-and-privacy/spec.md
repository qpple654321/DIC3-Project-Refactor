# Rider Pacing and Location Privacy Specification

## ADDED Requirements

### Requirement: Private GPX processing
Raw rider GPX files MUST NOT be committed to a public Git repository. Sensitive departure/arrival regions SHOULD be removed from derived public tables. API credentials MUST remain outside version control.

#### Scenario: Export of a derived dataset
- **GIVEN** a GPX file contains home-area coordinates
- **WHEN** creating shareable segments
- **THEN** the raw file remains private and identifying route endpoints are excluded.

### Requirement: Moving-time quantile model
The pacing subsystem SHALL compute rider-specific segment **moving-time** estimates using road geometry and terrain features, including P50, and conservative trip-level P90 estimates calibrated on held-out trips. It MUST NOT invert speed P90 and label it time P90.

#### Scenario: Estimating a route
- **GIVEN** an ordered segmented route and rider history
- **WHEN** pacing is requested
- **THEN** the model returns estimated moving times, ETA windows, units, model version, and trip P90 calibration metadata.

### Requirement: Group-aware performance validation
The evaluation SHOULD use GroupKFold split by ride identifier and out-of-time ride validation to avoid segment leakage.

#### Scenario: Two segments from one journey
- **GIVEN** two records from the same trip
- **WHEN** splitting training and evaluation
- **THEN** both MUST remain in the same split.
