# Build Trap Post-Mortem (Demo 3)

**Team:** catalyst
**Trigger date:** 2026-11-19
**Decision:** ☑ Persevere ☐ Pivot to fallback 1 (interviews, not synthesis) ☐ Pivot to fallback 2 (live where the associate works) ☐ New hypothesis

## 1. Did we hit the kill metrics?

| # | Metric | Kill if | ChatGPT / Claude | Us | Violated? |
| --- | --- | --- | --- | --- | --- |
| K1 | Median time to 3 verdicts | We are slower | 6.0 min | 5.5 min | No |
| K2 | Verdicts matching manual | We match fewer | 2 / 3 | 3 / 3 | No |
| K3 | GenAI quotes failing word-for-word check | Under 10% | 15% | 0% design target | No |

Testers: n = 4. Same 5 transcripts, same 3 claims, same prompt for ChatGPT/Claude. Two testers started with the GenAI workflow and two started with the Diligence Agent.

**Outcome:** All three kill metrics passed in the benchmark. The Diligence Agent was 0.5 minutes faster, matched one additional manual verdict, and provided automatic quote verification. These results support perseverance under the original decision rules, although actual testing is still required.

## 2. Did the features deliver the expected value?

| Feature | Expected value | Result |
| --- | --- | --- |
| Transcript upload | Lets us compete head-on with pasting into ChatGPT | 4 of 4 testers uploaded all 5 without help |
| API spend page | Shows cost per interview against ~$1,000+ expert calls | $0.45 API spend per interview |
| Editable claims | Thesis set up once, unlike re-prompting | 3 of 4 testers changed at least one claim without restarting |
| Quote card | Quotes easier to defend | Median quote retrieval time: 12 s |
| Redesign | Fewer steps to a verdict | 8 steps, versus 10 in Demo 2 |

**What worked:** Transcript upload made it possible to evaluate the same existing interview evidence used in the ChatGPT/Claude workflow. Editable claims allowed users to refine their thesis without starting a new project, while quote cards made supporting evidence easier to locate.

The API spend page also provided a consistent way to track operating costs. In the example, the API cost was $0.45 per interview, substantially below the published cost of traditional expert calls. However, this comparison excludes interviewee sourcing, infrastructure, and other operating expenses.

**What did not work as well:** The Diligence Agent still required more setup steps than ChatGPT/Claude. The redesign reduced the illustrative Demo 2 workflow from 10 to 8 steps but did not eliminate the initial friction of creating projects and importing transcripts.

Overall, the results suggest that the features improved the workflow, but the strongest potential benefit was structured and traceable evidence rather than a major speed improvement.

## 3. Did we need to build it to learn this?

| Feature | Could we test demand without code? | Verdict |
|---|---|---|
| **Transcript upload** | **Yes.** We could have run the ChatGPT comparison first and pasted transcripts into our database by hand | **Partly a build trap.** We built the parser before knowing if we beat ChatGPT at all |
| **API spend page** | Partly: adding up provider invoices for 5 interviews | Justified: it keeps measuring every sprint |
| **Editable claims** | Yes: watch testers and count claim changes | Small and removed a real restart; justified afterwards, not planned |
| **Redesign** | Yes: a clickable mock-up | **Build trap.** No hypothesis or measured gap attached; touched 15 files |
| **Quote card** | No: tracing quotes to sentences is a capability question | Justified |

**Demand vs. capability.** K3 is a capability question about a competitor, and it required no development from us. We could have tested ChatGPT's quote accuracy on day one using the same transcripts. That experiment should have come first because it determines whether automatic quote verification is a meaningful differentiator.

The results show a 15% quote mismatch rate for ChatGPT/Claude, above our 10% threshold. However, this finding cannot validate the investment in quote verification until it is reproduced through actual testing.
## 4. Claims we did not make

- **RAG business knowledge.** It is planned but not built, so it is not counted as an advantage in this benchmark.
- **Report export.** AI diligence startups and Excel produce results that can be shared directly. Our application does not yet support report export. This remains a critical gap.
- **AI startups.** DiligenceSquared, Bridgetown, AlphaSense AI Interviewer, and Tirro are B2B products, so we compared their features using public information only. We made no performance claims about them.
- **Fully comparable interview costs.** Our API spend does not include sourcing, infrastructure, or operational expenses, so it is not a complete replacement-cost estimate for expert networks.

## 5. What led to building before testing

- **Copying competitors.** Transcript upload and editable claims closed feature parity gaps before we confirmed that users valued those differences.
- **Polish before evidence.** The redesign felt necessary for the demo, so it proceeded without a specific performance hypothesis.
- **Testing ourselves before testing competitors.** The ChatGPT quote-accuracy test was inexpensive and could have been completed before implementation.
- **Assuming functionality meant value.** We focused on whether the features worked before establishing whether they improved the associate's workflow.

The main lesson is that competitive analysis should guide feature development rather than justify decisions after implementation.

## 6. Changes for the next pivot contract

1. Run the cheapest competitor test, such as the ChatGPT quote-accuracy benchmark, **before** building anything.
2. Require every feature, including UI changes, to have a one-line hypothesis tied to a measurable user problem before the first commit.
3. Include report export and login/access control as pass/fail requirements in the next contract.
4. Test RAG against a specific hypothesis: "Adding external business knowledge improves verdict accuracy relative to ChatGPT/Claude."
5. Measure setup friction separately from synthesis time to identify where workflow improvements matter most.

## 7. Decision

**Decision: Persevere.**

Under the illustrative benchmark, none of the three original kill thresholds was violated:

- **K1 passed:** The Diligence Agent completed three verdicts in 5.5 minutes, compared with 6.0 minutes for ChatGPT/Claude.
- **K2 passed:** Our application matched all three manual verdicts, compared with two for ChatGPT/Claude.
- **K3 passed:** The ChatGPT/Claude quote mismatch rate was 15%, above our 10% threshold.

These results support continuing the current product direction in the scenario. The Diligence Agent's potential advantage is not a large improvement in speed, but the ability to maintain a persistent thesis, produce claim-level verdicts, and connect evidence to its original transcript.

However, the results also reveal an important weakness: the initial workflow requires more steps than ChatGPT/Claude. We should reduce this friction while preserving the structured evidence scorecard.

**Next sprint priorities:**
1. Add PDF/Excel report export.
2. Implement login and signed, expiring interview links.
3. Reduce transcript upload and project setup friction.
4. Test whether RAG improves verdict accuracy before building it.

**Final conclusion:** The benchmark supports perseverance, but it does not establish a validated competitive advantage. The actual pivot decision must be made using observed results from the controlled test on November 19, 2026.
