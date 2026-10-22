Proof of Concept: Evidence Lender
1. What We're Proving
Our bet is that bank transaction history can provide useful evidence of a borrower's repayment capacity, even when a conventional credit file is thin. For Assignment 4, we test three claims:
1. Technical feasibility: an applicant uploads a bank statement CSV; the app extracts traceable cash-flow evidence, computes an experimental risk score when the data qualify, and lets a reviewer issue a decision.
2. Architectural clarity: the frontend, API/application logic, statement parser, experimental model, and decision/pricing components have separate responsibilities.
3. User viability: a borrower and reviewer can complete the primary journey. We document actual timings and friction in baseline-cuj.md once observed.
The scope is deliberately narrower than automated underwriting: the model does not generate an approved amount or interest rate, and no credit report is analyzed yet.
2. Why Cash Flow
Credit history does not always fully describe current financial circumstances. Cash-flow evidence—such as recurring inflows, spending, account balances, and volatility—may provide additional context. Research from the CFPB and FinRegLab motivates this approach; it does not validate our model or establish that we outperform credit scores.
3. Training Data and Model
Our experimental cash-flow-only Logistic Regression model uses the historical Berka / PKDD'99 Czech banking dataset, which links bank transactions to loan outcomes. Although the original source contains 682 loans, our modeling sample is 233 completed loans (203 repaid, 30 not repaid) after filtering. We label completed status A = 0 and B = 1; ongoing statuses C and D are excluded. Features are calculated only from transactions before each loan's start date, over a 180-day window.

The model extracts 18 numerical features, including inflows, outflows, monthly net cash flow, volatility, history coverage, transaction balances, and recent large inflows. Preprocessing and the fitted model were trained offline. The application uses exported model parameters with the same feature definitions, rather than retraining on every upload.

We selected the cash-flow-only Logistic Regression using training-set 3-fold cross-validation (mean ROC-AUC 0.843). On a 59-loan holdout set with eight defaults, ROC-AUC was 0.757, average precision 0.418, and Brier score 0.177. The bootstrap ROC-AUC interval was approximately 0.581–0.912. These results are preliminary because the sample is small, historical, and unlike today's Canadian borrowers. The score is not a calibrated Canadian PD.
4. System Topology
See [`system-topology.jpg`](system-topology.jpg).

Borrower UI (Next.js) → Node API → statement CSV parser and evidence extraction → experimental model (reviewer-facing) → human decision → borrower outcome. Pricing is a separate configuration-based module; application state is held in memory. The parser produces source-linked facts and states where information is missing or unverified. API boundaries keep the user interface separate from data processing and decisions. A future credit-report parser and jointly validated model could be added without replacing the whole borrower journey.
5. Loan Terms and Pricing (Illustrative)
The current prototype does not use a PD-based pricing formula. Its pricing configuration uses a fixed illustrative 24.9% APR over 18 months. A reviewer selects an amount within the configured limits and records a decision and reason. The app calculates corresponding monthly payment and total borrowing cost. It does not automatically approve borrowers or calculate a personalized rate from the experimental score. Future risk-based pricing would require validated PD estimates, loss assumptions, affordability checks, and legal review.
6. Validation Scope
Primary interaction: a borrower submits a bank statement CSV; the application displays extracted financial evidence to a reviewer; the reviewer accepts or declines with a written reason; the borrower can view the recorded outcome.

Working means:
1. Valid CSVs yield computed, source-linked financial facts; the experimental score appears only when its stricter input requirements are met.
2. Malformed uploads receive a clear error rather than a fabricated result.
3. Reviewer decisions and reasons are saved in application state and shown to the borrower.
4. Different statement contents change the extracted facts; any model output comes from actual fitted parameters, not a hardcoded number.
In scope now	Deferred
Single-account CSV parsing and evidence review	Bank API verification and multi-account reconciliation
Experimental cash-flow-only LR score for reviewers	Credit report analysis and validated combined PD
Human reviewer decisions	Automated underwriting and personalized pricing
Illustrative loan quote	Production security, persistence, and real lending


Known limitations: statements are uploaded rather than independently verified; descriptions and balances are optional for evidence extraction, but the model needs sufficient dated transactions and balances. The historical model was trained in CZK and may behave misleadingly on CAD inputs. Applicants are not shown the experimental score. State is in memory; production-grade authentication, privacy, data retention, and lending compliance are not implemented. Live timing and full browser-to-browser validation remain to be recorded, not assumed successful.
7. Pass Criteria and Evidence
Claim	Pass if	Current evidence/status
Technical feasibility	Valid CSV → review → recorded decision → borrower outcome, without placeholder core logic	Implemented in source; live end-to-end demo still to verify and capture
Model functionality	Exported cash-flow model computes reproducible outputs from eligible statements	Fitted parameter export and historical holdout metrics documented; Canadian validity unproven
Architectural clarity	Diagram agrees with actual UI/API/parser/model/decision boundaries	Diagram provided; verify against running system during demo
User viability	Complete measured CUJ with timing, context switches, screenshots, and Go/No-Go thresholds	Pending actual test in baseline-cuj.md


Go/No-Go: report the final result only after running the end-to-end demo and measuring the CUJ. A failed or unverified claim must be listed as a limitation, not counted as passed.
Sources
- CFPB — Credit scores only tell part of the story
- FinRegLab — The Use of Cash-Flow Data in Underwriting Credit
- Berka / PKDD'99 dataset

## Sources

- [CFPB (2023): Credit scores only tell part of the story](https://www.consumerfinance.gov/archive/blog/credit-scores-only-tells-part-of-the-story-cashflow-data/)
- [FinRegLab (2019): The Use of Cash-Flow Data in Underwriting Credit](https://finreglab.org/wp-content/uploads/2023/12/FinRegLab_2019-07-25_Research-Report_The-Use-of-Cash-Flow-Data-in-Underwriting-Credit_Empirical-Research-Findings.pdf)
- [Berka / PKDD'99, CTU Relational Learning Repository](https://relational.fel.cvut.cz/)
