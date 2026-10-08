# Cloud architecture and deployment — A3

The Classroom Presentation Randomizer is a browser-based tool for instructors to draw presentation teams without repeats and manage presentation/Q&A timers.

- [Live application](https://randomizer-weld-one.vercel.app)
- [Backend health](https://randomizer-smj8.onrender.com/api/health)
- [Application code repository](https://github.com/OliviaY1/randomizer)

## Artifact index

| Artifact | Location and status |
| --- | --- |
| Cloud architecture diagram | **Missing from this repository.** Add the team's actual diagram using standard cloud iconography and formal data/control-flow notation. The sample under `/architecture/diagram.md` is not the application diagram. |
| Architecture rationale | [rationale.md](rationale.md) — current topology, MVP trade-offs and proposed future evolution |
| A2 feedback reflection | [reflections.md](reflections.md) — existing 532-word team reflection, preserved from the merged A3 documentation PR |
| Deployment workflow explanation | [workflow.md](workflow.md) — trigger, configuration, execution, evidence and remaining verification |
| A2 CUJ evidence | [framework](../cuj/framework.md) and [lessons learned](../cuj/lessons-learned.md) |

## Verified release state

The application workflow has been merged into its `main` branch. GitHub validation passed frontend build/lint, all 24 backend tests including PostgreSQL, and all 8 deployment-status tests.

The [first main deployment run](https://github.com/OliviaY1/randomizer/actions/runs/37716917941) **failed before provider deployment** because all five required Actions settings were missing. The existing public app was reachable in the audit, but that does not establish a successful deployment through the new workflow. See [workflow.md](workflow.md).

The diagram and successful deployment evidence must be added before describing this A3 snapshot as complete. [Issue #10](https://github.com/lukezhang01/catalyst/issues/10) tracks release preparation. Documentation changes require teammate review and merge before finalizing the release snapshot.

## Submission

Publish the A3 Release in this no-code team repository, using an immutable semantic-version tag such as `v1.0.0-A3`. Its body should link the final artifacts, summarize progress and issue activity, and explain strategic changes. One team member submits the direct **Release URL** to Quercus.

The presentation PDF and standalone 30–60 second promotional video are separate A3 submissions. The supplied instructions do not require an Actions screenshot or recording. A link to a successful production run is useful supporting evidence.
