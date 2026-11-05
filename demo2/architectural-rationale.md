# Architectural Rationale (Demo 2)

**Team:** catalyst
**Compared with:** the October 24 prototype (snapshot `f212a70`), the starting point after our pivot to the diligence agent.
**Diagram:** [`evolved-topology.jpg`](evolved-topology.jpg) (green = new, orange = changed, red = removed).

## 1. Projects moved from browser storage to Supabase Postgres

**What changed.** Projects used to live in the associate's browser (`localStorage`). They are now stored in Postgres with five tables: `projects`, `interviewees`, `claims`, `interviews` and `evidence`. The browser loads and saves through `GET/PUT/DELETE /api/projects`, and each save is a single transaction.

**Why.** The scorecard (F4) has to update when an interview ends, but the interview runs in a *different* browser (the interviewee's). With browser storage, the transcript reached the project only if the associate's tab happened to be open and polling. Now the server saves the transcript and re-runs validation itself (`finishInterview`), whoever has a tab open.

**Alternatives.** (a) Keep browser storage and poll harder: still fails when the associate's tab is closed. (b) A document store (Firebase): simpler saves, but evidence is a link between a claim and one sentence of one interview, which is a relational shape. We chose Postgres so `evidence` can reference both `claims` and `interviews` with foreign keys and store the `turn_id` of the exact sentence quoted, which F5 needs.

**Technical debt.** Introduced: saves replace the whole project, so with two editors the last save wins. Plans and the next-round proposal sit in a `workspace` JSON column instead of tables. Resolved: data no longer disappears when a browser is cleared, and the team can share one project.

## 2. One AI provider instead of two

**What changed.** Question writing, findings and next-round planning moved from OpenAI to Claude. Claude Opus 5.5 does analysis; Claude Haiku 5.5 updates the question list during a live interview, where speed matters.

**Why.** Two providers meant two keys, two response formats and two sets of failure handling. The live question updates already used Claude.

**Alternative.** Keep both and pick per task. We dropped it because we saw no quality difference that justified the extra code.

## 3. Quotes are checked against the transcript on the server

**What changed.** `/api/thesis-validation` now drops any quote that doesn't appear word for word in one interviewee turn, records which turn it came from, and counts the dropped quotes.

**Why.** A made-up quote in front of an investment committee would ruin the product's credibility. Checking on the server means no client can skip it.

## 4. Repo stripped to one application

We removed the Python Pipecat and LiveKit demos, the debug UI and unused UI components. CI now runs one backend job and one frontend job (tests, type check, build) on every push and pull request.

## Architecture health check

**Healthy signs**
- New features go through API routes. The browser never touches the database, and Row Level Security blocks Supabase's public API, so only the backend can read the data.
- Server routes are tested on their own: `projects.test.ts` runs against an in-memory Postgres (PGlite).
- All API keys stay on the server; the interviewee's browser gets a short-lived voice token.

**Warning signs, with a fix plan**
- **Server imports frontend code.** `server/projects.ts` imports helpers from `frontend/src/lib`. A frontend change can break the backend. *Fix:* move shared types and helpers into a `shared/` package.
- **Live sessions are in memory.** A server restart loses interviews that are still running. *Fix:* store session state in Postgres or Redis.
- **No login.** Anyone with an interview link can read that session's config, which includes the thesis. *Fix:* signed, expiring links and accounts for associates.
- **Stale UI text.** The Settings page still says projects are stored "in this browser only".

## Limitations for future sprints

Phone interviews (Twilio), report export, CRM/network import, per-save conflict handling, and user accounts.
