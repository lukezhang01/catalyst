# Deployment workflow

## Source and trigger

The workflow lives in the separate application repository: [.github/workflows/deploy.yml](https://github.com/OliviaY1/randomizer/blob/9af47f9c68a07f9ac35bb981877979d305e2ef76/.github/workflows/deploy.yml). That link is pinned to the merged implementation commit.

- PRs targeting `main` run validation only.
- A push to `main`, normally a merged PR, runs validation and then production deployment.
- Manual dispatch on `main` supports retries after setup changes. Production does not run from other branches.

The A3 submission Release belongs in this team repo. Creating that Release does not itself trigger the application workflow.

## Execution

1. Build/lint the frontend; test the backend using SQLite and isolated PostgreSQL; test deployment-status handling.
2. Check required configuration and reject an outdated main commit.
3. Pull the existing Vercel production settings and build the frontend before changing either live service.
4. Ask Render to deploy the workflow commit. Poll until that deployment is live, rejecting provider failures, timeouts and mismatched commit IDs.
5. Check backend health and anonymous-access rejection.
6. Deploy the prebuilt frontend through Vercel, wait for completion, and check the public homepage and sign-in callback. Record the deployment information in the Actions summary.

Production runs are serialized. A failed validation job prevents the Actions production job. A Render failure stops frontend publication. A later Vercel failure can leave the API newer than the frontend; this is not an atomic cross-provider release.

## Configuration and secret handling

Actions references five repository Secrets: `VERCEL_TOKEN`, `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID`, `RENDER_API_KEY` and `RENDER_SERVICE_ID`. Token values must not appear in code, Issues, release notes or screenshots. IDs are non-secret identifiers but are read through the same configuration mechanism. PostgreSQL credentials stay in the backend host's runtime settings.

The application [setup guide](https://github.com/OliviaY1/randomizer/blob/9af47f9c68a07f9ac35bb981877979d305e2ef76/docs/deployment.md) covers provider settings and the steps required to activate the workflow. Existing provider Git integrations must be configured appropriately if Actions is to be the production gate; otherwise independent deployments can bypass its checks.

## Verification evidence and current limitation

| Evidence | Verified result |
| --- | --- |
| [PR validation run](https://github.com/OliviaY1/randomizer/actions/runs/37712127018) | Frontend build/lint, 24 backend tests including PostgreSQL, and 8 deployment-status tests passed. Production correctly skipped for a PR. |
| [First merged-main run](https://github.com/OliviaY1/randomizer/actions/runs/37716917941) | Validation passed. **Production failed at the configuration check:** all five settings listed above were missing. No provider deployment was attempted by this run. |
| [Public frontend](https://randomizer-weld-one.vercel.app) and [backend health](https://randomizer-smj8.onrender.com/api/health) | The earlier HTTP audit found them reachable, with the backend reporting PostgreSQL and sign-in configured. This is evidence of existing hosting, not a successful push through Actions. |

**A successful automated cloud push remains unverified.** After configuring the five values, run the workflow on the latest `main` and add that successful production run's URL here through a reviewed PR.

The team must also verify real sign-in, saving/reloading, loading from another device, account separation and persistence after redeployment. Automated health checks and tests with synthetic tokens do not establish those real-world results.

## Assignment evidence

A3 requires a working Actions cloud push and this technical summary with a direct workflow-file link. The supplied instructions do not explicitly require an Actions screenshot or recording. A successful run link is useful supporting evidence. The promotional video is a separate user-focused deliverable.

Related records: [application Issue #6](https://github.com/OliviaY1/randomizer/issues/6), [merged application PR #7](https://github.com/OliviaY1/randomizer/pull/7), and [team release-preparation Issue #10](https://github.com/lukezhang01/catalyst/issues/10).
