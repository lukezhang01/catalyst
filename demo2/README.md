Demo 2: Core Features & Proof of Value

Team: catalyst Product: AI diligence agent. Associates on a due diligence team enter an investment thesis, the AI interviews customers and experts through a browser link, and an evidence scorecard shows which claims are supported, contradicted or unclear, with the quotes behind each verdict.

Live app: https://diligence-frontend.dilligence-agent.workers.dev/ Code repository: https://github.com/OliviaY1/dilligence-agent

Artifacts
File	What it contains
product-definition-cuj.md	Persona, goal, and the full 10-step journey with friction type and status for each step
feature-prioritization.md	Feature-to-value mapping (job, pain, hypothesis, metric, priority), the sprint plan and mid-sprint changes
evolved-topology.jpg	System diagram showing what is new, changed and removed since the October prototype
architectural-rationale.md	Why we made each architecture change, the alternatives, the technical debt, and the health check
pivot-contract.md	Hypothesis, kill metric, trigger date (2026-11-05) and two fallback options, written before the sprint
build-trap-postmortem.md	Test results, whether building was needed to learn it, and changes for the next contract
What works end to end in this release
Create a project from a thesis; it is split into claims.
Claude writes opening questions for each interviewee; the associate edits them.
Plan who to interview and which claims each person covers.
Send an interview link. The interviewee talks to an AI voice interviewer in the browser, and the AI adds follow-up questions as it goes. The associate can watch live.
When an interview ends, the transcript is saved and the evidence scorecard updates on its own.
Each claim shows its status and quotes. Every quote is checked against the transcript.
Findings list research gaps and suggest a next round.

Not in this release: phone calls, report export, CRM/expert network import, user accounts. See feature-prioritization.md.

Sample data

Press Load sample projects on the home page to open the Maple Ridge Veterinary Group demo, which has every step filled in.
