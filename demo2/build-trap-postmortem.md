# Build Trap Post-Mortem (Demo 2)

**Team:** catalyst
**Trigger date:** 2026-11-05
**Decision:** ☐ Persevere ☑ Pivot to fallback 1 ☐ Pivot to fallback 2 ☐ New hypothesis

## 1. Did the features deliver the expected value?

**Pivot contract result (F4 scorecard)**

Test data: Maple Ridge Veterinary Group demo project, same 5 transcripts and 3 claims (Client Loyalty, Pricing Power, Vet Staffing) for every tester.

| Tester | Time with scorecard | Under 10 min? | Verdicts matching manual (of 3) | Pass? |
|---|---|---|---|---|
| T1 | 7.5 min | Yes | 3 / 3 | Yes |
| T2 | 9.0 min | Yes | 2 / 3 | No |
| T3 | 8.0 min | Yes | 3 / 3 | Yes |
| **Pass rate** | | | | **67% (kill if < 70%) → violated** |

Baseline: 20 minutes to read 5 transcripts and judge 3 claims by hand.

**Speed passed easily** (average 8.2 min, 59% faster than baseline). **Accuracy failed** for one tester on one claim.

**Other feature metrics**

| Feature | Success metric | Result | Met? |
|---|---|---|---|
| F1 Question generation | 80% of questions rated relevant | 17 of 20 (85%) | Yes |
| F2 AI interview link | 80% of test interviews (n ≥ 6) finish without breaking | 5 of 6 (83%) | Yes |
| F3 Interview plan | Every claim has ≥ 1 interviewee | 3 of 3 claims covered | Yes |
| F5 Source-linked quotes | Supporting quote found in under 10 s | 6 s average | Yes |

**Root cause of the miss:** T2 accepted the scorecard's "supported" verdict on Pricing Power. The manual verdict was "unclear". The supporting quotes all came from two current, loyal clients; the one former client who left over price was in a different round. Claude's verdict was technically backed by real quotes but the sample was one-sided. T2 trusted the badge and did not read the quotes. This is a capability problem (the AI's judgement), not a demand problem: all three testers said they would use a per-claim summary.

The one broken interview (F2) was a microphone permission failure on Safari. Logged as a bug, not a hypothesis failure.


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
- **F10 Sample mix check.** We deprioritized it as P2. The Pricing Power miss is exactly the failure it was meant to catch, so it moves up (see section 6).
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

**Pivot to Fallback 1: less automation, same problem.**

The kill metric was violated (67% < 70%), so per our contract we pivot and do not lower the threshold. The data points to Fallback 1 rather than Fallback 2: demand for a per-claim summary held up (all testers were faster and said they'd use it), but trusting the AI's verdict led a tester to a wrong answer.

**New hypothesis (Sprint 3):** If the scorecard groups verified quotes under each claim, labelled by interviewee type (current client, former client, expert), and the user sets the verdict themselves, users still finish at least 50% faster than baseline and match the manual verdict on all 3 claims.

- **Kill metric:** fewer than 70% of testers (n ≥ 5) finish in under 10 minutes with all 3 verdicts matching.
- **Trigger date:** 2026-11-19
- **Build plan:** remove the AI status badge from the scorecard, add interviewee-type labels to each quote (the first part of F10), keep the server-side quote check.
