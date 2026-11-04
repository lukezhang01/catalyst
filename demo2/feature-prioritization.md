# Feature Prioritization (Demo 2)

**Team:** catalyst
**Product:** AI diligence agent. Tests investment thesis claims through AI-run interviews.
**Persona:** associate on a commercial due diligence team.
**Baseline:** we pivoted from Evidence Lender, so the new baseline is the manual workflow (spreadsheet planning, expert network calls, reading transcripts by hand).

## Product Definition CUJ

| Step | User action | Feature |
|---|---|---|
| 1 | Enter thesis claims, review interview questions | F1 |
| 2 | Add interviewees and see who covers which claim | F3 |
| 3 | Send interview links; AI runs the interviews | F2 |
| 4 | See which claims are supported, contradicted or unclear | F4 |
| 5 | Click a verdict to see the exact quotes behind it | F5 |
| 6 | Run another round for weak claims | Deferred |
| 7 | Export findings for the investment committee | Deferred |

## Feature-to-Value Mapping

Jobs (J) and pains come from our Value Proposition Canvas.

| Feature | Job | Pain relieved | Hypothesis | Success metric | Priority |
|---|---|---|---|---|---|
| **F1 Question generation** | J1 Turn claims into testable questions | Unclear which questions test each claim | AI questions are usable with light edits | 80% rated relevant by testers | P0 |
| **F2 AI interview link** | J2 Schedule interviews, J3 Run interviews | Days of scheduling; ~$1,000+ and analyst time per call | People complete an AI interview on their own time | 80% of test interviews (n ≥ 6) finish without breaking | P0 |
| **F3 Interview plan** | J4 Decide who to talk to next | Same views repeated while key claims stay untested | Mapping people to claims shows gaps | Every claim has at least 1 interviewee | P1 |
| **F4 Evidence scorecard** | J5 Pull evidence together per claim | Notes scattered; synthesis by hand | Users judge claims faster than reading transcripts | See [pivot contract](pivot-contract.md) | P0 |
| **F5 Source-linked quotes** | J6 Present to the investment committee | Findings may not hold up under questioning | Linking each verdict to quotes makes it defensible | Testers find the supporting quote in under 10 s | P1 |

## Implementation Plan

| When | What | Owner |
|---|---|---|
| This sprint | F2, backend, deployment | Luke |
| This sprint | F1, F4 (AI parts), F3 | Olivia |
| This sprint | Frontend, F5, baseline and user testing | David |
| Later | Next-round planning, sample mix check, CSV/CRM import, phone calls, report export, database | TBD |

**Why this order:** F1, F2 and F4 are the smallest set that goes from claims to evidence, and F2 and F4 are the riskiest, so they start first. Phone calls are deferred because an interview link tests the same value with far less setup.