# Architecture rationale

This describes the implemented system and its technical trade-offs. The [A2 CUJ](../cuj/framework.md) and [team reflection](reflections.md) provide the feedback context. Where the original decision-making context is not documented, questions are retained rather than inventing a historical justification.

## Current topology

- A user's browser loads the React/Vite frontend from Vercel over HTTPS. Team selection, timers and editing run in the browser.
- Signed-out setups are stored in that browser's `localStorage`.
- Microsoft sign-in supplies identity information. Signed-in clients call the FastAPI backend on Render over HTTPS to load or replace their account's setup.
- The backend verifies the supplied token and uses a tenant/user identifier to select the user's saved setup in PostgreSQL. The production database provider and plan are not established by the repository and need team confirmation.
- CORS restricts browser API access to the configured frontend origin. Runtime credentials remain in provider settings; deployment credentials are referenced through GitHub Secrets.

User interactions and saved-setup requests are the application's **data flows**. Reviewed repository changes, GitHub Actions validation, Render deployment requests and Vercel publishing are **control flows**. The required diagram should distinguish these paths and the browser, identity provider, frontend host, API host and persistent database boundaries.

## Immediate MVP choices

**Hosted access removes a documented entry barrier.** The A2 audit reports approximately 20 minutes spent configuring a local Python/Node environment. A shareable deployed URL lets an instructor access the app without that local setup. This addresses the reported friction; no new measured time-saving benchmark is claimed here.

**Browser interactions stay responsive without a server call for every timer tick or draw.** React owns the classroom interaction flow while the API focuses on authenticated persistence. Independent deployment is possible, at the cost of coordinating API URLs, CORS, authentication and payload compatibility.

**Keep the existing Vercel/Render services during the deployment-automation change.** These hosts already serve the frontend and API. Reusing them keeps the current change focused on delivery. Their original selection criteria, evaluated alternatives, pricing and availability guarantees require team confirmation.

**Local saving supports account-free use; PostgreSQL supports saved account setups.** Local storage is convenient for one browser but can be cleared and is not cross-device storage. A hosted database separates account data from the API instance's ephemeral filesystem. SQLite supports local development behind the same storage interface. One JSON setup per user matches the current save/load operation, but provides neither multiple named sessions nor versioned edit history.

**Microsoft sign-in avoids maintaining application passwords.** Per-user identifiers support data separation. The current implementation uses ID tokens at the API; an access-token authorization design remains a technical follow-up. This document does not certify the authentication configuration or real-account journeys as fully verified.

**A lean Actions workflow deploys the tested main commit.** PR validation precedes the production job triggered by a merge/push to main. It builds the frontend, waits for the requested backend commit to become live, then publishes the frontend. It does not provide an atomic release across hosts; API changes must remain backward compatible. Details and the current failed-production-run status are in [workflow.md](workflow.md).

## Feedback response and remaining product work

The existing [532-word reflection](reflections.md) records instructor feedback to communicate user value more clearly and explains the move from a local prototype to a hosted experience.

The A2 release also prioritized bulk CSV/JSON roster import to address its reported 15-minute manual-entry bottleneck. The inspected application still uses manual team entry; bulk import is **not claimed as delivered**. The team should confirm whether it is deferred and document the reason. Account saving helps reuse a prepared roster but does not remove first-time manual entry.

## Future evolution

Possible next steps are bulk import, multiple saved sessions, conflict detection/history, improved API authorization, tested database recovery, monitoring, and scaling services in response to measured demand. These are proposals rather than approved commitments. Multi-region infrastructure and shared live editing are not established MVP requirements. Any change to primary use cases needs the instructor approval specified by the course.

## Questions for the team

1. What originally motivated Vercel and Render, and which alternatives were actually evaluated?
2. Which provider/plan hosts PostgreSQL, and what are the documented backup, recovery, sleep and expiry constraints?
3. Which Microsoft account types should work, and which have been tested successfully?
4. Why was bulk import deferred from the A2 priorities, if it remains deferred?
5. Have the primary use cases changed, and if so where is instructor approval recorded?
6. Which browser and persistence checks were completed, on what dates, with what results?

Implementation references: [application decision record](https://github.com/OliviaY1/randomizer/blob/9af47f9c68a07f9ac35bb981877979d305e2ef76/docs/architecture-decisions.md), [storage](https://github.com/OliviaY1/randomizer/blob/9af47f9c68a07f9ac35bb981877979d305e2ef76/backend/storage.py), and [setup persistence](https://github.com/OliviaY1/randomizer/blob/9af47f9c68a07f9ac35bb981877979d305e2ef76/frontend/src/useSetupStore.js).
