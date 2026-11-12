# 03 — Introduction (Refactored Academic Draft)

## I. INTRODUCTION

Large language models (LLMs) have made natural-language itinerary generation increasingly accessible. Nevertheless, generating a plausible sequence of destinations is not equivalent to producing an itinerary that satisfies interacting spatial, temporal, operational, and safety-related constraints. TravelPlanner [1] demonstrates how stringent real-world planning evaluation can expose failures that conventional language-quality assessments overlook. For motorcycle touring, the feasibility gap is especially consequential: travel-time variability is rider-specific, mountain weather can change along the route, sunset imposes a practical deadline, and fuel stations may be too sparse or closed when reached.

The proposed **Moto Touring Copilot** investigates a more precise research question: **Can an LLM-generated motorcycle itinerary be validated as executable under rider-specific pacing, daylight, fuel autonomy, weather hazards, and changing road conditions?** This is a feasibility evaluation problem rather than solely a recommendation application. It requires specifying measurable constraints, computing conservative arrival-time intervals, and independently checking compliance instead of accepting an LLM's self-assessment.

### A. Problem Formulation
Let a candidate itinerary include an ordered road-segment sequence, a departure time, refueling stops, rest allowances, rider-specific model parameters, and source-stamped environmental observations. A candidate is feasible only if **every applicable hard rule** passes: R1 daylight/return deadline, R2 fuel reachability and opening hours, R3 severe weather time-window policy, R4 road restrictions, and R5 numerical provenance. Predictions should report uncertainty, and a failure to retrieve essential conditions must not be silently interpreted as a pass.

### B. Proposed Method
The architecture separates language-based understanding and candidate generation from deterministic numerical computation. Five specialized agents coordinate planning, personalized pacing, weather assessment, road/fuel acquisition, and verification. Historical private GPX traces inform segment moving-time quantiles, and held-out ride conformal calibration supports conservative whole-trip time estimates. Weather and supply queries use predicted **arrival windows**, not just the time at which the user asks a question. A bounded **Generate–Verify–Repair** procedure either yields an externally checked candidate or returns an explicit infeasibility report with violations and possible modifications.

### C. Research Contributions
This study **proposes**, rather than yet demonstrates: (1) a motorcycle-specific constrained sequential-planning formulation and traceable feasibility taxonomy; (2) an integrated multi-agent, tool-grounded and deterministic verification/replanning design; and (3) reproducible comparisons using an independent oracle and rider-specific pacing baselines. The outcome may also inform other physical endurance-limited itineraries, although cross-domain generalization is beyond the immediate evaluation.

### D. Hypotheses and Scope
H1 tests external verification against LLM-only planning. H2 isolates data grounding. H3 tests replanning under simulated or historical disruption. The primary scope is **pre-departure decision support**; continuous in-ride tracking, MQTT and spoken alerts are L3 extensions. The system does **not** attempt to predict accidents or claim a guarantee of rider safety.

### E. Paper Organization
Related Work reviews travel benchmarks, hybrid verifiers, adaptive planning and evidence grounding. System Design specifies agents, tools, uncertainty and hard rules. Experimental Design formalizes E1–E4, oracle independence, metrics, and limitations.

## Key references for verification
[1] Xie et al., *TravelPlanner: A Benchmark for Real-World Planning with Language Agents*, ICML 2024, https://arxiv.org/abs/2402.01622.  
Additional source citations and claims must be checked against the original proposal and publications before academic submission.
