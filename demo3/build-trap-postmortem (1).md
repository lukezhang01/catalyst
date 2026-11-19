# Build Trap Post-Mortem (Demo 3)

**Team:** catalyst
**Trigger date:** 2026-11-19
**Decision:** ☐ Persevere ☐ Pivot to fallback 1 (interviews, not synthesis) ☐ Pivot to fallback 2 (live where the associate works) ☐ New hypothesis

> **Fill in after testing:** every `[ ]` below. The thresholds in `pivot-contract.md` stay as written.

## 1. Did we hit the kill metrics?

| # | Metric | Kill if | ChatGPT / Claude | Us | Violated? |
|---|---|---|---|---|---|
| K1 | Median time to 3 verdicts | We are slower | [ ] min | [ ] min | [ ] |
| K2 | Verdicts matching manual | We match fewer | [ ] / 3 | [ ] / 3 | [ ] |
| K3 | GenAI quotes failing word-for-word check | Under 10% | [ ]% | 0% by design | [ ] |

Testers: n = [ ]. Same 5 transcripts, same 3 claims, same prompt for ChatGPT/Claude.

**Root cause of any miss:** [ ]
*(Likely place to look for K1: uploading 5 files one by one and confirming the speaker each time, compared with one paste into a chat.)*

## 2. Did the features deliver the expected value?

| Feature | Expected value | Result |
|---|---|---|
| Transcript upload | Lets us compete head-on with pasting into ChatGPT | [ ] of [ ] testers uploaded all 5 without help |
| API spend page | Shows cost per interview against ~$1,000+ expert calls | $[ ] per interview |
| Editable claims | Thesis set up once, unlike re-prompting | [ ] |
| Quote card | Quotes easier to defend | Quote found in [ ] s |
| Redesign | Fewer steps to a verdict | [ ] steps, vs [ ] in Demo 2 |

## 3. Did we need to build it to learn this?

| Feature | Could we test demand without code? | Verdict |
|---|---|---|
| **Transcript upload** | **Yes.** We could have run the ChatGPT comparison first and pasted transcripts into our database by hand | **Partly a build trap.** We built the parser before knowing if we beat ChatGPT at all |
| **API spend page** | Partly: adding up provider invoices for 5 interviews | Justified: it keeps measuring every sprint |
| **Editable claims** | Yes: watch testers and count claim changes | Small and removed a real restart; justified afterwards, not planned |
| **Redesign** | Yes: a clickable mock-up | **Build trap.** No hypothesis or measured gap attached; touched 15 files |
| **Quote card** | No: tracing quotes to sentences is a capability question | Justified |

**Demand vs. capability.** K3 is a pure capability question about a competitor, and it needed **no code from us at all**: we could have tested ChatGPT's quote accuracy on day 1. That test should have come first, because it decides whether our main differentiator exists.

## 4. Claims we did not make

- **RAG business knowledge.** It's planned but not built, so it isn't counted as an advantage in this benchmark.
- **Report export.** The AI startups and Excel all produce something to hand over; we don't yet. This is a critical gap, not shared parity.
- **AI startups.** DiligenceSquared, Bridgetown, AlphaSense AI Interviewer and Tirro are B2B, so we compared features from public information only and made no performance claims about them.

## 5. What led to building before testing

- **Copying competitors.** Upload and editable claims closed parity gaps before we checked that users cared.
- **Polish before evidence.** The redesign felt necessary for a demo, so it skipped the pivot contract.
- **Testing ourselves before testing the competitor.** The ChatGPT test costs nothing and should have shaped what we built.

## 6. Changes for the next pivot contract

1. Run the cheapest competitor test (here, ChatGPT) **before** building anything.
2. Every feature, including UI work, gets a one-line hypothesis tied to a measured gap before the first commit.
3. Put report export and login in the next contract as pass/fail requirements.
4. Test RAG with a hypothesis ("verdicts match the manual verdict more often than ChatGPT") before building it.

## 7. Decision

[ ] *(Persevere / fallback 1 / fallback 2 / new hypothesis, and why, based on K1–K3.)*
