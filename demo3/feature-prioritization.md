# Feature Prioritization & Gap Analysis (Demo 3)

**Team:** catalyst
**Product:** AI diligence agent. It tests investment thesis claims through AI-run interviews or uploaded transcripts.
**Persona:** associate on a commercial due diligence team.

## 1. Who we compete with

| Group | Competitor | How the associate works with it | Tested how |
|---|---|---|---|
| **A. Traditional** | Expert network (GLG) + Excel | Book expert calls, attend them, tag findings per claim in a spreadsheet | **Hands-on** (we did the synthesis step by hand); call cost and lead time from published data |
| **B. Traditional + GenAI** | ChatGPT / Claude | Paste the transcripts and the thesis into a chat and ask for an analysis | **Hands-on** |
| **C. AI diligence startups** | DiligenceSquared, Bridgetown Research, AlphaSense AI Interviewer, Tirro | Done-for-you or enterprise platforms that run interviews and deliver a report | **Public information only.** All are B2B, sold through sales calls, so we could not run them |

Groups A and B are the two competitors we ran with an identical journey. Group C is used for feature parity and positioning only, and every figure from it is marked **(pub.)**.

## 2. Competitive Review CUJ

**Same persona:** an associate testing an investment thesis.
**Same input:** the same 5 Maple Ridge Veterinary Group transcripts and the same 3 claims (client loyalty, pricing power, vet staffing).
**Same success criteria:** each claim marked supported, contradicted or unclear, each verdict backed by at least one quote that appears **word for word** in a transcript, and the result matches the team's manual verdict.

| Step | A. GLG + Excel | B. ChatGPT / Claude | Diligence Agent |
|---|---|---|---|
| 1. Get interviews | Book through the network; associate attends | Needs transcripts from somewhere else | AI interview by link, or upload a transcript |
| 2. Set up the thesis | Claim columns typed into Excel | Thesis pasted into the prompt every time | Thesis split into editable claims once, saved with the project |
| 3. Analyze | Read each transcript, copy quotes into cells | One prompt; answer depends on prompt wording | Scorecard per claim, updates on its own |
| 4. Check quotes | Associate's own copying | Must search each quote in the transcript by hand | Quotes not found word for word are dropped before display |
| 5. Add a 6th interview | Re-read and update cells | Paste everything again, re-prompt | Upload it; scorecard re-runs |
| 6. Share the result | Excel / slides | Copy the chat answer | **No export yet** |

## 3. Performance Ratios

| Metric | A. GLG + Excel | B. ChatGPT / Claude | Diligence Agent | Comparison |
| --- | --- | --- | --- | --- |
| Time to 3 verdicts (5 transcripts) | 20 min (Demo 2 baseline) | 6.0 min (sim.) | 5.5 min (sim.) | 3.64x faster than A; 1.09x faster than B |
| Steps / clicks to 3 verdicts | 13 (sim.) | 5 (sim.) | 8 (sim.) | 1.60x as many steps as B |
| Verdicts matching manual assessment | 3/3 (reference) | 2/3 (sim.) | 3/3 (sim.) | +1 correct verdict vs B |
| Quotes found word for word | 100% (manual reference) | 85% (sim.) | 100% (design target) | +15 percentage points vs B |
| Steps to add a 6th interview | 5 (sim.) | 4 (sim.) | 3 (sim.) | 25% fewer steps than B |
| Cost per interview | ~$1,000–1,500 (pub.) | Subscription; interviews not included | $0.45 API spend (sim.) | ~2,222–3,333x lower API-only cost |
| Time to first interview | 24 h to 1–2 weeks (pub.) | N/A; needs existing transcripts | 0.5 h (sim.) | Not directly comparable without equivalent sourcing |

**How to run B:** a new chat, the same 5 transcripts, and one fixed prompt for every tester: *"Here is a thesis with 3 claims and 5 interview transcripts. For each claim, say supported, contradicted or unclear, and give the exact quotes that show it."* Then check every quote against the transcripts.

## 4. Feature Parity


| Capability | A. GLG + Excel | B. ChatGPT / Claude | C. AI startups (pub.) | Us |
| --- | --- | --- | --- | --- |
| Runs interviews | Human calls | No | AI voice interviews | Yes |
| Finds interviewees | Yes | No | Some providers offer expert sourcing | No |
| Thesis → claims → verdict per claim | By hand | Requires prompting | Hypotheses and reports | Yes, saved and editable |
| Quotes traced to transcripts | By hand | Requires manual verification | Transcript-linked evidence offered by some providers | Yes, checked automatically |
| Updates when new interview arrives | By hand | Re-prompt | Not verified | Yes, automatically |
| Upload existing transcripts | — | Yes | Not verified across vendors | Yes |
| Business knowledge beyond transcripts | Associate's own | General model knowledge | Bridgetown includes secondary research | Not yet (RAG planned) |
| **Report export** | Excel / slides | Copy answer | Yes, diligence reports | **No** |
| Cost visible per project | Credits, often unclear | Subscription | Pricing varies; ~$50K per project reported for DiligenceSquared | Yes |
| Login and access control | Yes | Yes | Yes | **No** |

## 5. Critical vs. Acceptable Gaps

### Critical gaps (block adoption)

| Gap | Why it is critical | Decision | Target |
|---|---|---|---|
| **No report export** | Every competitor ends in something the associate can hand to the investment committee: Excel, a chat answer or a full report. We end on a screen | Next sprint: export the scorecard with claims, verdicts and quotes as PDF and Excel | Export in 1 click; testers don't need to re-type anything |
| **No login or access control** | Deal teams won't put a confidential thesis into a tool where a link is enough to read it | Next sprint: accounts plus signed links that expire | Every project and link requires access |
| **Must win against ChatGPT / Claude** | It's free or already paid for, and associates already use it. If we aren't faster or more accurate, there's no reason to switch | Measured by the pivot contract this sprint | See K1–K3 |

### Acceptable gaps

| Gap | Why it does not threaten adoption |
|---|---|
| No interviewee sourcing | Associates can upload calls from the networks they already pay for. In customer diligence, the deal team usually has the contact list |
| Not a done-for-you service (DiligenceSquared, Bridgetown) | They sell a finished report at about $50K per project. We are a self-serve tool for the associate, at a different price point and for a different buyer |
| No RAG business knowledge yet | Our claim-level verdicts work from transcripts alone; RAG would improve them but isn't required to run the journey |

### Differentiation we emphasize

1. **Thesis-first and persistent.** ChatGPT answers one prompt. We keep the thesis, claims and evidence together and update them as each interview arrives.
2. **Every quote verified.** Our check drops any quote that isn't in the transcript word for word. With ChatGPT, the associate has to check each quote by hand (15% failed the check in our test).
3. **Interviews and synthesis in one place.** ChatGPT can't run the interview; GLG + Excel can't analyze it.

## 6. Strategic Feature Decisions

| Feature | Decision | Gap it addresses | Owner | Status |
|---|---|---|---|---|
| **F11 Transcript upload** (.txt, .vtt, .srt, .md, Otter; confirm the interviewee) | **Add** | Lets us compete head-on with "paste into ChatGPT" | Olivia | Done |
| **F12 API spend per project** | **Add** | Measures the cost advantage over expert calls | Olivia | Done |
| **F13 Editable claims** | **Add** | The thesis is set up once and can change, unlike re-prompting | Luke | Done |
| **F5 Quote card** (every quote and its source per claim) | **Finish** | Strengthens differentiation 2 | Olivia | Done |
| Redesign (light theme, projects grouped by status) | **Modify** | Fewer steps to a verdict | Olivia | Done |
| **F7 Report export** (PDF / Excel) | **Next sprint** | Critical gap | David | Planned |
| **F14 Business knowledge via RAG** | **Next sprint, test first** | Hypothesis: verdicts match the manual verdict more often than ChatGPT | David | Not built |
| Login and signed links | Next sprint | Critical gap | David | Planned |
| Interviewee sourcing | **Omit** | Acceptable gap | — | Not planned |

## Sources (published competitor data)

- AlphaSights ~$1,000–1,500 per call: https://www.userintuition.ai/compare/alphasights-vs-user-intuition/
- Expert network turnaround 24 h to 1–2 weeks: https://cleverx.com/blog/expert-networks-explained-how-companies-connect-with-industry-specialist/
- DiligenceSquared (AI interviews, traceable reports, ~$50K per project): https://www.ycombinator.com/launches/Om9-diligencesquared-ai-powered-market-due-diligence-for-investment-decisions and https://techcrunch.com/2026/03/05/diligencesquared-uses-ai-voice-agents-to-make-ma-research-affordable/
- Bridgetown Research (AI voice interviews through expert networks, first analysis in 24 h): https://techcrunch.com/2025/02/26/bridgetown-research-raises-19m-to-speed-up-due-diligence-with-ai
- AlphaSense AI-led expert calls: https://help.alpha-sense.com/hc/en-us/articles/50627969088019-AI-Led-Expert-Calls-FAQs
