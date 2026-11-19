# Pivot Contract (Demo 3)

**Team:** catalyst
**Written:** before sprint work began (see this file's commit date)
**Carried over from Demo 2:** Persevere. Our core interview-to-scorecard workflow demonstrated initial value, so we continued developing the Diligence Agent while shifting our focus toward competitive benchmarking and differentiation.

## Context

We compare the Diligence Agent with what an associate would otherwise use:

- **A. Traditional:** expert network calls (GLG) with findings organized in Excel.
- **B. Traditional + GenAI:** pasting transcripts into ChatGPT or Claude.
- **C. AI diligence startups** (DiligenceSquared, Bridgetown Research, AlphaSense AI Interviewer, Tirro): B2B, so we use public information only.

We already beat A on time in Demo 2. **B is the real threat**: it is cheap, fast and already on every associate's laptop. This contract tests whether we beat it.

## Hypothesis

For the same input (5 Maple Ridge transcripts, 3 claims), associates using the Diligence Agent reach their verdicts **no slower than with ChatGPT/Claude**, match the manual verdict **at least as often**, and get quotes that are **100% traceable** to a transcript. With ChatGPT/Claude, at least 10% of quotes fail a word-for-word check.

In short: **same speed as GenAI, but verdicts you can defend.**

## Kill Metrics

We pivot if **any** of these is true on the trigger date:

| # | Metric | Kill threshold | How we measure it |
|---|---|---|---|
| K1 | Median time from 5 transcripts to 3 verdicts | **Slower than ChatGPT/Claude** | Same testers do both, half starting with each; stopwatch |
| K2 | Verdicts matching the manual verdict | **Fewer than ChatGPT/Claude** | Out of 3 claims per tester |
| K3 | Share of ChatGPT/Claude quotes that fail a word-for-word check | **Under 10%** (then quote checking isn't a real advantage) | Search every quote in the transcripts |

Each threshold is fixed before testing and will not be adjusted afterwards. We use the same fixed prompt for ChatGPT/Claude for every tester (in `feature-prioritization.md`).

## Trigger Date

**2026-11-19** (Demo 3)

## Fallback Options

1. **Interviews, not synthesis.** If ChatGPT/Claude analyzes transcripts as well as we do, our synthesis isn't the product. We would focus on the part GenAI chats can't do: running AI interviews with the target's customers from a contact list, and handing over clean transcripts.
2. **Live where the associate already works.** If the claim scorecard is valuable but a separate app is too much friction, we deliver it as an Excel scorecard or a Claude/ChatGPT add-on that keeps the thesis and claims between sessions. This tests whether the value is the *structure*, not the *platform*.

If a kill metric is violated, we either pivot to one of these options or write a new hypothesis with a new threshold and trigger date.
