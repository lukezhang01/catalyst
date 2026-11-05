# Product Definition CUJ (Demo 2)

**Team:** catalyst
**Product:** AI diligence agent. It tests investment thesis claims through AI-run interviews.

This is the whole journey the product has to support, not only what we built this sprint. Status shows where each step stands at this release.

## Persona

**Associate on a commercial due diligence team** (private equity or consulting). They get an investment thesis from the deal team, for example "Maple Ridge Veterinary Group can raise prices 5% a year without losing clients", and have 2 to 4 weeks to say whether the evidence supports it.

Today they plan in a spreadsheet, book calls through expert networks (GLG, AlphaSights, Third Bridge) at about $1,000+ per call, sit in every call, and then read all the transcripts by hand to build the evidence for the investment committee.

## Value-driven goal

> Go from a written thesis to a defensible verdict on each claim, backed by real quotes, without scheduling, attending or rereading every interview.

**Success for the user:** for every claim they can say *supported, contradicted or unclear* and point to the sentence that shows it.

## The journey

Friction types: **Blocker** = step cannot be done without a feature. **Friction** = possible but slow or painful. **Integration** = user has to leave the product. **Connection** = two steps don't pass data to each other.

| Step | User action | Today (manual) | Friction type | Feature | Status |
|---|---|---|---|---|---|
| 1 | Enter company and thesis; review the claims it is split into | Claims live in a memo; nobody writes down what evidence would prove each one | Friction | F1 | Built |
| 2 | Review the interview questions for each claim | Associate writes questions from scratch for every call | Friction | F1 | Built |
| 3 | Add interviewees and see which claims each person covers | Spreadsheet; key claims often get no interviewee | Connection | F3 | Built |
| 4 | Import contacts from a CRM or expert network | Copy names and numbers by hand | Integration | F9 | Deferred |
| 5 | Send an interview link; the AI runs the interview and asks follow-ups | Days of scheduling; associate attends every call | Blocker | F2 | Built (browser link, not phone) |
| 6 | Watch a live interview, or end it early | Sit in the call | Friction | F2 | Built |
| 7 | See which claims are supported, contradicted or unclear | Read every transcript, tag quotes by hand | Friction (the biggest one) | F4 | Built |
| 8 | Check the exact quotes behind each verdict | Search transcripts for the line you remember | Friction | F5 | Partly built |
| 9 | See research gaps and plan the next round for weak claims | Guess who to call next | Connection | F6 | Built, not tested |
| 10 | Export findings for the investment committee | Copy quotes into slides by hand | Integration | F7 | Deferred |

## Features the journey needs

| ID | Feature | Journey steps |
|---|---|---|
| F1 | Claim and question generation (Claude writes opening questions per person from the thesis; editable) | 1, 2 |
| F2 | AI interview link (voice interview in the browser, live follow-up questions, live view for the associate) | 5, 6 |
| F3 | Interview plan (each interviewee mapped to the claims they test) | 3 |
| F4 | Evidence scorecard (status per claim; refreshes on its own after each interview) | 7 |
| F5 | Source-linked quotes (every quote checked against the transcript and named by speaker) | 8 |
| F6 | Research gaps and recommended next round | 9 |
| F7 | Report export for the investment committee | 10 |
| F8 | Phone interviews (Twilio) | 5 |
| F9 | Contact import (CSV, CRM, expert networks) | 4 |
| F10 | Sample mix check (warns if one type of interviewee dominates a claim, e.g. all current customers) | 3, 7 |

Feature-to-value mapping, priorities and the sprint plan are in [`feature-prioritization.md`](feature-prioritization.md).
