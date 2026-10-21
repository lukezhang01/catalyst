# Baseline CUJ & Audit: TODO (product name)

**Team:** Catalyst
**Date of audit:** Oct 20, 2026

## 1. Overview

A private credit associate gets a borrower's bank transaction export on Monday and needs a rate recommendation she can defend at the investment committee (IC) by Wednesday.

Our product takes the borrower's transactions as a CSV, and our ML model outputs two things:

1. A **credit score**
2. A **suggested interest rate** based on that score

The current version does **not** explain why a borrower got that score. Reasons are planned for a future iteration.

This document measures the journey on the product **as it is today** (the baseline). After we make changes, we rerun the same journey and compare against these numbers.

---

## 2. Persona and User Goal

### Persona: Private credit associate at a mid-market private equity firm

| | |
|---|---|
| **Role** | Associate on the private credit (lending) team of a mid-market PE firm |
| **Day to day** | Screens 5-15 prospective borrowers per week, works mostly in Excel, writes IC memos |
| **What they start with** | The borrower's bank transactions exported as CSV. Each bank uses its own layout (e.g. separate MoneyIn / MoneyOut columns vs. one signed Value column, different date formats). |
| **Constraints** | Tight deal deadlines. Every number in the IC memo must be justified. A score with no reasoning can't go in the memo. |
| **What they care about** | Speed to a decision, being able to defend the rate, consistency across deals |

### User Goal

> Turn a borrower's bank transaction CSV into a credit score and suggested interest rate that they can **defend** in an IC memo, in **under 10 minutes**.

- **Measurable:** end-to-end time of 600 seconds or less
- **Concrete outcome:** a score, a rate, and a justification ready to paste into the IC memo

---

## 3. Journey Audit

### Audit setup

- **Tester:** TODO (name and role, ideally someone not on the dev team)
- **Product version / commit:** TODO
- **Test borrower data:** TODO (which sample CSV, which layout, number of transactions, date range)
- **Start point:** borrower's CSV is on the desktop, app is open on the login page
- **End point:** score, rate, and a justification are in the IC memo template
- **How measured:** screen recording + stopwatch, one screenshot per step

### Friction severity scale

- **None / Great:** completes the step with no confusion
- **Moderate:** hesitates or hits unclear UI, but recovers without help
- **Severe:** abandons the step, needs outside help, or gets clearly frustrated

### Audit table

> The actions below are a draft of the expected flow. TODO: edit them to match what the tester actually did.

| Step # | Action (what the user does) | Context Switches (where the user loses focus or leaves the app) | Time (s) | Friction Severity | Evidence |
|---|---|---|---|---|---|
| 1 | Open the app and log in | TODO | TODO | TODO | ![Step 1](TODO-screenshots/step1.png) |
| 2 | Click to start a new credit assessment | TODO | TODO | TODO | ![Step 2](TODO-screenshots/step2.png) |
| 3 | Enter borrower details (TODO: confirm fields, e.g. company name, loan amount, term) | TODO | TODO | TODO | ![Step 3](TODO-screenshots/step3.png) |
| 4 | Upload the borrower's bank transaction CSV | TODO | TODO | TODO | ![Step 4](TODO-screenshots/step4.png) |
| 5 | Run the model and wait for results | TODO | TODO | TODO | ![Step 5](TODO-screenshots/step5.png) |
| 6 | Read the credit score | TODO | TODO | TODO | ![Step 6](TODO-screenshots/step6.png) |
| 7 | Read the suggested interest rate | TODO | TODO | TODO | ![Step 7](TODO-screenshots/step7.png) |
| 8 | Try to work out why the score is what it is (the app shows no reasons yet) | TODO | TODO | TODO | ![Step 8](TODO-screenshots/step8.png) |
| 9 | Get the score, rate, and a justification into the IC memo | TODO | TODO | TODO | ![Step 9](TODO-screenshots/step9.png) |

### Totals

| | Baseline |
|---|---|
| Total time (s) | TODO |
| Total context switches | TODO |
| Steps rated Severe | TODO |
| Steps rated Moderate | TODO |
| Completed without outside help? | TODO (Yes / No) |

---

## 4. Highlights and Lowlights

> Rows below are what we expect to find based on the draft flow. TODO: confirm, edit, or replace each one with what was actually observed.

| Severity | Type | Step # | Finding | Proposed Fix |
|---|---|---|---|---|
| Severe | Lowlight | 8 | The app shows a score and rate but no reasons. User leaves the app (e.g. scrolls through the CSV in Excel, asks a teammate) to guess why, or can't justify the number at all. TODO: confirm | Show the top factors behind each score in plain language |
| Severe | Lowlight | 4 | The borrower's bank CSV doesn't match the layout the app accepts (different column names, single Value column vs. MoneyIn / MoneyOut, or a different date format). User leaves the app to fix the file in Excel. TODO: confirm | Auto-detect common bank CSV layouts and show a column-mapping preview |
| Moderate | Lowlight | 5 | No feedback while the model runs. User is unsure if it's working. TODO: confirm | Add a progress indicator with an estimated wait time |
| Moderate | Lowlight | 9 | No export. User screenshots and retypes the score and rate into the memo. TODO: confirm | Add an "Export summary" button |
| None / Great | Highlight | 6, 7 | Score and suggested rate appear together on one results page. TODO: confirm | Keep as is |
| None / Great | Highlight | TODO | TODO | Keep as is |

---

## 5. Product Recommendations

### Recommendation 1: "Why this score?" panel

- **Pain point:** Step 8, TODO seconds, rated TODO. User leaves the app to TODO (where they went to figure out the reason).
- **Technical change:**
  - For each prediction, compute how much each input factor pushed the score up or down.
  - Return the top 5 factors from the backend together with the score and rate.
- **Design change:**
  - Add a panel under the score that lists each factor in plain language, with its direction and size. Example: "Frequent overdrafts in the last 3 months lowered the score by 38 points."
- **Expected impact:** removes the context switch at step 8. Step time drops from TODO s to under 60 s. Severity goes from TODO to None.

### Recommendation 2: Flexible bank CSV upload

- **Pain point:** Step 4, TODO seconds, rated TODO. User leaves the app to TODO (e.g. rename columns or fix dates in Excel).
- **Technical change:**
  - Accept both common layouts:
    - 5 fields: MoneyIn, MoneyOut, Date, Description, Reference
    - 4 fields: Value (positive = in, negative = out), Date, Description, Reference
  - Match column headers to these fields even when names differ (e.g. "Amount" → Value, "Credit" → MoneyIn, "Debit" → MoneyOut).
  - Auto-detect the date format (DD/MM/YYYY vs. MM/DD/YYYY) from the data.
  - If something is wrong, show an error that names the exact row and column.
- **Design change:**
  - Add a mapping preview screen after upload. Matched columns get a check. Unmatched columns get a dropdown so the user can pick the right field.
  - Show the first 5 rows as the app reads them, so the user can confirm money in / money out is correct before running the model.
- **Expected impact:** removes the Excel context switch. Step time drops from TODO s to under 90 s. Severity goes from TODO to None or Moderate.

### Other candidates (if time allows)

- One-click export of score, rate, and reasons for the IC memo (step 9)
- Progress indicator during model run (step 5)
- Upload several CSVs at once for borrowers with more than one bank account

---

## 6. Success Metrics

The next iteration passes if every target below is met on the same journey with the same test data.

| Metric | Target (next iteration) | Baseline | How measured |
|---|---|---|---|
| End-to-end task time | 600 s or less | TODO | Stopwatch from login (step 1) to results in memo (step 9) |
| Steps rated Severe | 0 | TODO | Audit table |
| Steps rated Moderate | 2 or fewer | TODO | Audit table |
| Context switches | 1 or fewer (opening the borrower's CSV is allowed) | TODO | Audit table |
| Completed without outside help | Yes | TODO | Observation during audit |

> TODO: adjust targets with the team if needed.