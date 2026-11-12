# 04 — Related Work (Refactored Academic Draft)

## II. RELATED WORK

### A. LLM-Based Travel Planning Benchmarks
TravelPlanner [1] assesses whether natural-language agents produce itineraries that simultaneously satisfy practical constraints, moving beyond language fluency. ChinaTravel [2] broadens open-ended travel-planning evaluation. These benchmarks motivate complete-plan constraint satisfaction as a primary metric. The exact constraint coverage of each benchmark should be checked rather than asserting universal absence of motorcycle considerations without systematic evidence.

### B. Hybrid Planning and External Verification
Hybrid LLM/planner systems [3], TRIP-PAL [4] and LLM-Modulo [5] investigate separating natural-language generation from external planners, solvers or verification. This motivates a **Generate–Verify–Repair** loop in which numerical and physical constraints are checked by tools, not LLM self-critique. Literature-reported benchmark success rates are specific to their dataset and protocol; they are not observed results for Moto Touring Copilot.

### C. Dynamic and Disruption-Aware Itineraries
Flex-TravelPlanner [6] and TripTide [7] address adaptability under changed travel assumptions. Our proposal considers arrival-time-specific weather and road events, unavailable fuel stops and bounded replanning. A vital scope distinction is **pre-departure recalculation** versus continuous **in-ride real-time** monitoring, which is future work rather than an implemented capability.

### D. Source Grounding and Feasibility Validation
Itinerary verification research such as Iti-Validator [8] motivates guardrails, traceable numeric evidence and consistency checks. CWA forecasts and warnings, TDX road events, road-network data, topography and fuel-station hours are candidate official/grounded sources. Their quality, API availability and temporal precision must be reported, including explicit treatment of missing or stale results.

### E. Research Positioning

| Research stream | Representative works | Main emphasis | Proposed extension |
|---|---|---|---|
| General itinerary benchmarks | [1], [2] | Multi-constraint itinerary evaluation | Rider-specific physical constraints |
| Hybrid LLM planning | [3], [4], [5] | Generate + verify / solve | ETA-aligned terrain, daylight and fuel verification |
| Adaptive planning | [6], [7] | Changed assumptions, replanning | Road closures and service hours under physical constraints |
| Itinerary validation | [8] | Logic/temporal checks and guardrails | Reproducible independent oracle with numerical provenance |

The research gap is **the integration** of motorcycle-specific constraint formalization, personalized uncertain travel time, authoritative ETA-aligned observations, deterministic hard-rule verification, and replanning. Novelty of each component is not claimed individually. This integration remains an empirically testable research proposal.

## Bibliography — verify exact author/title/version before final submission
[1] Xie et al., *TravelPlanner*, ICML 2024. https://arxiv.org/abs/2402.01622  
[2] *ChinaTravel*. https://arxiv.org/abs/2412.13682  
[3] *LLMs Can Plan Your Travels Rigorously*. https://arxiv.org/abs/2404.11891  
[4] *TRIP-PAL*. https://arxiv.org/abs/2406.10196  
[5] *LLM-Modulo*. https://arxiv.org/abs/2405.20625  
[6] *Flex-TravelPlanner*. https://arxiv.org/abs/2506.04649  
[7] *TripTide*. https://arxiv.org/abs/2510.21329  
[8] *Iti-Validator*. https://arxiv.org/abs/2510.24719

References are supplied as research leads, not represented as fully audited bibliographic entries.
