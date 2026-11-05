# Build Trap Post-Mortem (Demo 2)

**Team:** catalyst
**Trigger date:** 2026-11-05
**Decision:** ☐ Persevere ☐ Pivot to fallback 1 ☐ Pivot to fallback 2 ☐ New hypothesis *(tick after testing)*

> **To fill in after testing on 2026-11-05:** every `[ ]` below. Do not change the threshold from `pivot-contract.md`.

## 1. Did the features deliver the expected value?

**Pivot contract result (F4 scorecard)**

| Tester | Time with scorecard | Under 10 min? | Verdicts matching manual (of 3) | Pass? |
|---|---|---|---|---|
| T1 | [ ] min | [ ] | [ ] / 3 | [ ] |
| T2 | [ ] min | [ ] | [ ] / 3 | [ ] |
| **Pass rate** | | | | **[ ]% (kill if < 70%)** |

Baseline: 20 minutes to read 5 transcripts and judge 3 claims by hand.

**Other feature metrics**

| Feature | Success metric | Result | Met? |
|---|---|---|---|
| F1 Question generation | 80% of questions rated relevant | [ ] | [ ] |
| F2 AI interview link | 80% of test interviews (n ≥ 6) finish without breaking | [ ] of [ ] | [ ] |
| F3 Interview plan | Every claim has ≥ 1 interviewee | [ ] | [ ] |
| F5 Source-linked quotes | Supporting quote found in under 10 s | [ ] s | [ ] |

**Where a metric was missed, root cause:** [ ]

## 2. Did we need to build it to learn this?

We split each question into **demand** (do associates want this outcome?) and **capability** (can we automate it?).

| Feature | Demand question | Could we test it without building? | Capability question | Verdict |
|---|---|---|---|---|
| F4 Scorecard | Do associates decide faster from a per-claim summary? | **Yes.** A hand-made scorecard in a spreadsheet from the same 5 transcripts (Wizard of Oz) would have answered it. | Can Claude produce verdicts that match ours? | **Partly a build trap.** Demand could have been tested on paper first. Only the capability half needed code. |
| F2 Interview link | Will people finish an interview with an AI? | Hardly. The experience of talking to an AI voice is the thing being tested. | Does the voice pipeline hold up for a whole interview? | **Build justified.** It was also mostly built in the October prototype, so the cost was low. |
| F1 Questions | Do associates use generated questions? | Yes. Paste Claude output into a doc and ask testers to rate it. | Can it be done per interviewee? | Cheap to build; low risk either way. |
| F5 Quotes | Does a quote next to a verdict raise trust? | Yes, with the same paper scorecard plus quotes. | Can quotes be traced to an exact sentence? | Capability needed code (quote check, `turn_id`). |
| F6 Next round | Do associates want the AI to plan the next round? | Yes. Show a written suggestion and ask. | — | **Build trap.** Built in October before anyone asked for it. Not tested this sprint. |
| Supabase database | — (not a user outcome) | — | Can the scorecard update when the associate isn't watching? | Necessary for F4 to work across two browsers. Not a demand test. |

## 3. Features not built this sprint

- **F8 Phone calls.** Deprioritized because a browser link tests the same demand ("will people talk to an AI?") with no telephony setup. We learned that the link alone covers the core loop.
- **F7 Report export.** Only matters if the scorecard is trusted, which is what the pivot contract tests. Building it first would have been premature.
- **F9 Contact import and F10 sample mix check.** They reduce friction in a journey we haven't validated yet.
- **F5 click-through to the transcript.** Data is stored; UI moved to next sprint so the time went into making the scorecard update after every call.

## 4. What led to building before testing

- **Momentum from the prototype.** After the pivot, we had a working voice pipeline from October and kept building on it. Having working code made building feel cheaper than testing, even for features (F6) whose demand we hadn't checked.
- **Demo pressure.** A live demo needs working software, so we treated "it runs" as "it's validated".
- **One hypothesis covered two questions.** Our pivot contract measures speed *and* accuracy of the scorecard together. If it fails we won't know whether associates don't want a summary (demand) or Claude's verdicts are wrong (capability).

## 5. Changes for the next pivot contract

1. Run a **paper or Wizard-of-Oz test first** for any demand question, and only build once it passes.
2. Write **separate kill metrics** for demand and capability (e.g. "a hand-made scorecard saves 50% of time" and "Claude matches our verdict on ≥ 80% of claims").
3. Use **more testers** (n ≥ 5) so a single tester can't swing the 70% threshold.
4. Any carried-over feature (like F6) gets a hypothesis before we spend more time on it.

## 6. Decision

[ ] *(Persevere / pivot to fallback 1 or 2 / new hypothesis with new threshold and trigger date, and why, based on the numbers above.)*
