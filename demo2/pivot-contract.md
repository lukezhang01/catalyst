# Pivot Contract (Demo 2)

**Team:** catalyst
**Written:** before sprint work began (see this file's commit date)

**Scope note:** This sprint, interviews run through a browser interview link instead of phone calls. Phone calling through Twilio is deferred because time was short: we pivoted from Evidence Lender to the diligence agent last week, after the Evidence Lender idea ran into too many security and privacy problems. The interview link tests the same value (an AI runs the interview, the associate does not) with much less setup.

## Hypothesis

With the evidence scorecard, users decide whether each thesis claim is supported **at least 50% faster** than by reading interview transcripts by hand, and they **reach the same verdict**.

- **Baseline (manual):** reading 5 interview transcripts and judging 3 thesis claims takes **20 minutes**.
- **Target:** under **10 minutes** for the same 5 interviews and 3 claims, using the scorecard and its quotes.

## Kill Metric

If **fewer than 70%** of testers (n ≥ 2) finish in **under 10 minutes** *and* match the manual verdict on **all 3 claims**, we pivot.

Both conditions count. A tester who is fast but gets a claim wrong counts as a failure, because a fast wrong answer is worse than a slow right one in diligence.

**How we measure it**

| Item | Method |
|---|---|
| Time | Stopwatch from opening the Findings page to stating a verdict for all 3 claims |
| Correct verdict | Compared with the verdict the team agreed on after reading all 5 transcripts by hand |
| Test data | The Maple Ridge Veterinary Group demo project, with the same 5 transcripts for every tester |

## Trigger Date

**2026-11-05**

On this date we compare the results with the kill metric and record a pivot or persevere decision in `build-trap-postmortem.md`. We will not change the 70% / 10-minute threshold after testing starts.

## Fallback Options

1. **Less automation, same problem.** The AI only groups transcript quotes under each claim; the user decides the verdict. This tests whether the value is in *organising* the evidence rather than in the AI's *judgement*. If testers don't trust the AI verdict, this is the version to test.
2. **Different part of the workflow.** Focus on interview planning (which claims need more evidence and who to interview next) instead of synthesis. This tests whether associates lose more time deciding *who to talk to* than *reading what they said*.

If the kill metric is violated, we either pivot to one of these options or write a new hypothesis with a new threshold and trigger date.
