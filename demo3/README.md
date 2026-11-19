# Demo 3: Competitive Review & Gap Analysis

**Team:** catalyst
**Product:** AI diligence agent. Associates enter an investment thesis, the AI interviews people through a browser link (or the associate uploads an existing transcript), and an evidence scorecard shows whether each claim is supported, contradicted or unclear, with every quote traced to its speaker.

**Live app:** https://diligence-frontend.dilligence-agent.workers.dev (frontend on Cloudflare Workers, backend on Render)
**Code repository:** https://github.com/OliviaY1/dilligence-agent
**Release tag:** `v1.2.0-A6`

## Artifacts

| File | What it contains |
|---|---|
| [`feature-prioritization.md`](feature-prioritization.md) | Competitive Review CUJ against expert networks + Excel and ChatGPT/Claude (hands-on), plus AI diligence startups (public info); performance ratios; feature parity; critical vs. acceptable gaps; feature decisions |
| [`evolved-topology.jpg`](evolved-topology.jpg) | System diagram showing what changed since Demo 2 |
| [`architectural-rationale.md`](architectural-rationale.md) | Why each architecture change was made, alternatives, technical debt, remaining limitations |
| [`pivot-contract.md`](pivot-contract.md) | Competitive-positioning hypothesis, kill metrics K1–K3, trigger date (2026-11-19) and two fallback options, written before the sprint |
| [`build-trap-postmortem.md`](build-trap-postmortem.md) | Results against the kill metrics, whether building was needed, and the decision |

## Competitive position in one line

Against the traditional workflow (expert calls + Excel) we are faster and cheaper. Against pasting transcripts into ChatGPT/Claude, we aim for the same speed but verdicts you can defend: **a saved thesis with a verdict per claim, and every quote checked word for word against the transcript.**

## New this sprint

1. **Transcript upload.** Import a Zoom/Teams `.vtt`, `.srt`, `.txt`, `.md` or Otter export, confirm which speaker is the interviewee, and it counts as a completed interview in the scorecard.
2. **API spend.** Every paid call is recorded with its model and list-price cost, shown per project and in total.
3. **Editable claims.** Add, edit or delete claims before the plan; add or reword them after interviews have started.
4. **Quote card.** Opening a claim shows its thesis sentence and every quote with its source.
5. **Redesign.** One light theme across the app; projects grouped by status.
6. **Automatic migrations.** The server applies database migrations on start.

## Known critical gaps (next sprint)

Report export (PDF / Excel); login and signed, expiring interview links. Business knowledge via RAG is planned but not built. Interviewee sourcing is deliberately out of scope (see `feature-prioritization.md`).

## Sample data

Press **Load samples** on the home page to open the Maple Ridge Veterinary Group demo.
