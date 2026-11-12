# DIC3 — Moto Touring Copilot | OpenSpec Refactor

**Status:** 2026-11-12, Proposal-stage documentation only. No application has been implemented or experimentally evaluated.

**Original project:** https://github.com/yotsubamomo/moto-touring-copilot  
**Instructor sample:** https://github.com/huanchen1107/2026-11-11-DIC3-ProjectRefactor

## DIC3 checklist

| Requirement | Evidence | Caveat |
|---|---|---|
| Learn and install OpenSpec | [Guide](docs/refactor/01-OPEN-SPEC-GUIDE.md) | Local installation/CLI execution not verified |
| A–F, 5 cores, 3 trade-offs, 10 details | [Research framework](docs/refactor/02-AF-5-3-10.md) | Proposed rather than measured |
| DESIGN.md → OpenSpec workflow | [Change proposal](openspec/changes/moto-tour-feasibility-refactor/proposal.md), [design](openspec/changes/moto-tour-feasibility-refactor/design.md), [tasks](openspec/changes/moto-tour-feasibility-refactor/tasks.md), five specs | Run CLI validation |
| Introduction refactor | [Introduction](docs/refactor/03-INTRODUCTION.md) | Academic draft |
| Related Work refactor | [Related Work](docs/refactor/04-RELATED-WORK.md) | Independently verify all citations |

**Canonical original:** [DESIGN.md](https://github.com/yotsubamomo/moto-touring-copilot/blob/main/DESIGN.md), [Proposal PDF](https://github.com/yotsubamomo/moto-touring-copilot/blob/main/docs/proposal/proposal.pdf), [Slides](https://github.com/yotsubamomo/moto-touring-copilot/blob/main/docs/proposal/proposal-slides.pptx).

## Local OpenSpec verification (PowerShell)

```powershell
node --version
npm install -g @fission-ai/openspec@latest
openspec --version
openspec list
openspec validate moto-tour-feasibility-refactor
```

The workflow here was authored as a proposal-stage starting point. CLI initialization and validation are **not** claimed complete. Check `openspec help` if the installed version changes its schema or command names.

## Source-of-truth and security

Do not publish raw, untrimmed rider GPX tracks, home/work coordinates, or API keys. L2 pre-departure advice is in scope; live ride-time MQTT/voice warning (L3) is future work. E1–E4 are **planned**, not observed results.
