# Architectural Rationale (Demo 3)

**Team:** catalyst
**Compared with:** Demo 2 (see [`../demo2/architectural-rationale.md`](../demo2/architectural-rationale.md)).
**Diagram:** [`evolved-topology.jpg`](evolved-topology.jpg). Orange boxes and arrows are the parts that cost money.

The design has two goals: keep every secret on the server, and pay only for model calls that produce evidence. The frontend runs as a Cloudflare Worker, the backend is one Render web service, and the data lives in Postgres (Supabase today, Render PostgreSQL later). Claude and Boson AI are the only paid APIs.

## 1. Keys stay on the server; browsers get short-lived tokens

**What it is.** The Boson, Anthropic and LiveKit keys exist only in the backend's environment. When an interviewee opens a call, the browser asks `GET /api/token`. The backend then mints a Boson client secret that expires after 600 seconds. LiveKit room tokens for the avatar are minted the same way, in `POST /api/avatar/start`.

**Why.** The call has to run in the interviewee's browser, which we do not control. Anything sent there can be read, so we send a token that dies within ten minutes instead of the Boson key itself. A leaked token gives no access to Claude, the database or LiveKit.

**Alternative.** Relay all audio through our server and never give the browser a token. We rejected it because the server would then carry every audio stream, adding a hop of latency and the bandwidth cost on Render (see section 4).

## 2. Only the backend can reach the database

**What it is.** The browser reads and writes projects through `/api/projects`. It never connects to Postgres. Every table has Row Level Security turned on with no policies, which blocks Supabase's public REST API. The backend connects as `postgres`, which bypasses RLS. The connection requires SSL unless the host is `localhost`.

**Why.** Supabase exposes every table over a public API by default. Without RLS, anyone holding the project's anon key could read all theses and transcripts.

**What it is (CORS).** When `FRONTEND_ORIGIN` is set, the API sends CORS headers only to that one origin. Pages on other sites cannot read the API's responses from a browser.

## 3. Schema changes apply themselves on start

**What it is.** On start the server runs `migrate()`. It applies each `supabase/migrations/*.sql` file once, records it in `schema_migrations`, and holds `pg_advisory_xact_lock(491)` inside one transaction. If two instances start together, only one of them migrates. If a migration fails, the server logs a warning and still starts.

**Why.** We plan to move from Supabase to Render PostgreSQL. With this runner, a new database needs only `DATABASE_URL`; nobody types schema into a vendor's web console, where a missed column would only show up as a runtime error. A database first set up by hand is detected (it has `projects` but no `schema_migrations`), and the first file is marked as already applied.

## 4. Audio goes straight from the browser to Boson

**What it is.** The voice call (Higgs Realtime) and the transcription run between the interviewee's browser and Boson. The avatar video comes from Boson through a LiveKit room. Our backend only relays small JSON: the call's config, its state, and the consultant's "end" request (`/api/interviews/:id/*`).

**Why.** A small Render instance can serve the whole app, because its traffic is text. Audio and video never touch it, so the backend's cost does not grow with call length.

**Limits that hold cost down.** Each avatar session is capped at 180 seconds, so a tab that crashes frees its slot (and stops billing) within three minutes. The server also ends its open avatar sessions on `SIGINT` or `SIGTERM`, which Render sends on each redeploy. The in-memory session list is capped at 200, and the oldest session is dropped first.

## 5. The cheapest model that does the job

| Task | Model | Price per 1M tokens (in / out) | Why this model |
|---|---|---|---|
| Live question updates during a call | Claude Haiku 5.5 | $0.10 / $0.50 | Runs many times per call while the interviewee is still talking |
| Interview questions, thesis validation, next-round plan | Claude Opus 5.5 | $4 / $20 | Runs a few times per project; its output goes in front of an investment committee |
| Voice | Higgs Realtime | $0.75 (cached $0.25) / $4.50 | The only realtime voice model we use |
| Transcription | Higgs STT 3.1 | $0.0025 per minute | |
| Avatar | Higgs Avatar | $0.07 per minute (480×640) | We use the default size; 640×640 costs $0.09 |

Haiku costs 40 times less than Opus per token. Using Opus for live updates would also make the interviewee wait.

Thesis validation does not run on every page view. The scorecard stores an `inputKey` built from the thesis, the claims and the completed transcripts, and Opus runs again only when that key changes.

An uploaded transcript (`.vtt`, `.srt`, `.txt`, `.md`) costs no voice, transcription or avatar minutes. It is parsed in the browser and counts as a completed interview, so the only paid call it causes is the next validation.

## 6. Every paid call is recorded with its cost

**What it is.** Each Claude call on the server writes a row to `api_usage` right after the reply arrives. The call's usage (voice tokens, transcription seconds, avatar minutes and Haiku tokens) is added up in the interviewee's browser. When the call ends, `finishInterview` saves it. Each row has the project, the use, the model, the token or minute counts and the list-price cost from `server/usage.ts`. The API spend page and each project's spend card read `GET /api/usage`.

**Why.** The vendors' dashboards show one total per API key. They cannot tell us which project or which feature spent the money.

**Decision.** Spend is shown, never enforced. A project over budget can still run calls, because cutting an interview off halfway wastes what it already cost.

**Alternative.** Read usage from the vendors' dashboards. Rejected because those dashboards report per API key, not per project, and Boson's avatar session has no duration field we could read back.

## Technical debt and remaining limits

- **No login.** CORS stops other websites from calling the API from a browser. It does not stop a script. Anyone who finds the backend URL can call `/api/thesis-validation` and spend our Claude budget. Fix: accounts for associates and a per-project rate limit.
- **Interview links are not signed.** A link is a 16-character random id (`randomBytes(8)`) with no expiry. Anyone holding it can read that session's config, which includes the thesis. Fix: signed links that expire.
- **Call cost is reported by the browser.** A modified browser could report lower usage. Usage that arrives after the call first ends is not counted (a `shortcut:` comment in `server/interviews.ts`). The numbers are for monitoring, not billing.
- **Prices are copied by hand.** `server/usage.ts` holds list prices from boson.ai/pricing and Anthropic. A model missing from the table is saved with no price and shown as "unpriced". When a vendor changes prices, this file has to be updated.
- **Not yet tested on Render PostgreSQL.** The migration runner is tested only against PGlite (in-memory Postgres). We have not run it against Supabase or Render's Postgres yet, and have not checked that Render's internal Postgres accepts the SSL setting in `server/db.ts`.
- **Live sessions are in memory** (carried over from Demo 2). A redeploy during a call loses that call.
