# 01 — OpenSpec Installation and Spec-Driven Development

## Goal
Use **spec-driven development (SDD)** to translate the source [DESIGN.md](https://github.com/yotsubamomo/moto-touring-copilot/blob/main/DESIGN.md) into a reviewable change: **proposal → design → capability specifications → tasks → implementation → verification → archive**.

## Prerequisites and installation
Node.js/npm and Git, plus an OpenSpec-supported coding environment. From repository root in PowerShell:

```powershell
node --version
npm install -g @fission-ai/openspec@latest
openspec --version
openspec help
openspec list
openspec validate moto-tour-feasibility-refactor
```

Refer to the [OpenSpec official repository](https://github.com/Fission-AI/OpenSpec) for version-specific CLI usage. For a fresh project, run `openspec init`; in this repository preserve pre-existing `openspec/` artifacts and inspect help before replacing any content.

## SDD workflow

1. **Source:** DESIGN.md FR1–FR8, NFR1–NFR4, Rules 1–5, Algorithm 1, E1–E4.
2. **Propose:** Define the user problem, scope boundaries, rationale and acceptance criteria.
3. **Design:** Choose agents, data flows, model calibration and fail-closed verification.
4. **Specify:** Encode SHALL/MUST requirements and Given/When/Then acceptance scenarios.
5. **Task:** Plan implementation and tests without pretending they are complete.
6. **Verify:** Run OpenSpec validation; implement tests and independently evaluate.
7. **Archive:** Only after implementation and demonstrated verification.

## Evidence to capture
Screenshot/copy `openspec --version`, `openspec list`, `openspec validate ...`, repository commit and tested rule coverage. **No CLI execution has been verified in this documentation-only submission.**

## Safety and scope
L2 pre-departure route assistance is the committed level. Do not claim accident predictions, real-time in-ride behavior, verified safety or measured performance. Raw GPS traces and API keys remain private.
