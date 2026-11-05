# Feature Prioritization (Demo 2)

**Team:** catalyst
**Product:** AI diligence agent. It tests investment thesis claims through AI-run interviews.
**Persona:** associate on a commercial due diligence team.
**Baseline:** we pivoted from Evidence Lender, so the new baseline is the manual workflow: spreadsheet planning, expert network calls, and reading transcripts by hand.

The full journey is in [`product-definition-cuj.md`](product-definition-cuj.md). The short version:

## Product Definition CUJ

| Step | User action | Feature |
|---|---|---|
| 1 | Enter thesis claims, review interview questions | F1 |
| 2 | Add interviewees and see who covers which claim | F3 |
| 3 | Send interview links; AI runs the interviews | F2 |
| 4 | See which claims are supported, contradicted or unclear | F4 |
| 5 | Check the exact quotes behind a verdict | F5 |
| 6 | Review gaps and run another round for weak claims | F6 |
| 7 | Export findings for the investment committee | F7 (deferred) |

## Feature-to-Value Mapping

Jobs (J) and pains come from our Value Proposition Canvas.

| Feature | Job | Pain relieved | Hypothesis | Success metric | Priority |
|---|---|---|---|---|---|
| F1 Question generation | J1 Turn claims into testable questions | Unclear which questions test each claim | AI questions are usable with light edits | 80% rated relevant by testers | P0 |
| F2 AI interview link | J2 Schedule interviews, J3 Run interviews | Days of scheduling; ~$1,000+ and analyst time per call | People complete an AI interview on their own time | 80% of test interviews (n ≥ 6) finish without breaking | P0 |
| F3 Interview plan | J4 Decide who to talk to | Same views repeated while key claims stay untested | Mapping people to claims shows gaps | Every claim has at least 1 interviewee | P1 |
| F4 Evidence scorecard | J5 Pull evidence together per claim | Notes scattered; synthesis by hand | Users judge claims faster than by reading transcripts | See pivot contract | P0 |
| F5 Source-linked quotes | J6 Present to the investment committee | Findings may not hold up under questioning | Linking each verdict to quotes makes it defensible | Testers find the supporting quote in under 10 s | P1 |
| F6 Gaps and next round | J4 Decide who to talk to next | Weak claims stay weak because nobody plans a follow-up | A suggested next round is accepted with light edits | 2 of 3 testers keep at least half the suggested people | P2 |
| F7 Report export | J6 Present to the investment committee | Copying quotes into slides by hand | An export saves the last manual step | Not set yet | P2 |
| F8 Phone interviews | J3 Run interviews | Some experts won't click a link | Phone reach raises completion rate | Not set yet | P2 |
| F9 Contact import | J2 Schedule interviews | Retyping contacts from a CRM or network | Import removes setup time | Not set yet | P2 |
| F10 Sample mix check | J5 Pull evidence together | One-sided evidence (only happy customers) | A warning stops false "supported" verdicts | Not set yet | P2 |

## Why this priority order

- **P0 = blocker.** Without F1, F2 and F4 there is no path from a thesis to evidence at all. F2 and F4 are also the riskiest assumptions (will people talk to an AI? will users trust an AI verdict?), so they were started first.
- **P1 = big friction.** F3 and F5 work around steps the user *can* do by hand but slowly. F5 matters because a verdict nobody can trace to a quote won't survive an investment committee.
- **P2 = improves a working journey or adds reach.** None of these blocks the core loop, so they wait until the pivot contract tells us the core loop is worth keeping.

## Implementation Plan

| When | What | Owner | Status at release |
|---|---|---|---|
| This sprint | F2 (interview link, live view), backend, deployment | Luke | Done |
| This sprint | F1, F4 (AI parts), F3 | Olivia | Done |
| This sprint | Supabase database (projects, claims, interviews, evidence), evidence update after every call | Olivia | Done |
| This sprint | Frontend, F5, baseline and user testing | David | F5 partly done; testing on 2026-11-05 |
| Carried over | F6 gaps and next round (built in the October prototype, kept in the app) | — | Works; not tested with users |
| Later | F7 report export, F8 phone calls, F9 CSV/CRM/network import, F10 sample mix check, user accounts | TBD | Not started |

**Why this order:** F1, F2 and F4 are the smallest set that goes from claims to evidence. Phone calls (F8) are deferred because an interview link tests the same value with far less setup.

## Mid-sprint changes (technical honesty)

- **Database moved up.** It was on the "later" list. We moved it into this sprint because the scorecard has to update when an interview ends, and the interviewee's tab cannot write into the associate's browser storage. Storing projects on the server fixed that (see `architectural-rationale.md`).
- **F5 is only partly done.** The scorecard shows up to two quotes per claim, each checked against the transcript and named by speaker. The planned "click a verdict to jump to the exact sentence in the transcript" is not in the UI yet. The data for it is already stored (`evidence.turn_id`), so it is a frontend task for next sprint.
- **F6 exists but was not prioritized.** It came from the October prototype and still works. We did not test it, so we make no claim about its value yet.
- **Expert networks are placeholders.** GLG, AlphaSights and Third Bridge appear in the UI but are not connected. Contacts are entered by hand.
