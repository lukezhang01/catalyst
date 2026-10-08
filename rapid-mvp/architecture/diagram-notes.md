# Diagram scope, notation and sources

- [PNG preview](diagram.png) — 2700 × 1995, readable on a light background.
- [Scalable SVG](diagram.svg).
- [Editable diagrams.net document](diagram.drawio) — open/import in diagrams.net. Components, labels and connectors are editable; this is a documentation artifact, not application code.

## Meaning

The runtime panel shows the logical deployment topology inferred from the inspected application code and public HTTP endpoints. Blue arrows represent requests; responses use the same connections. Frames identify browser/provider/service boundaries, not verified private networks. The numbered paths are asset delivery, Microsoft sign-in, authenticated setup requests, signing-key retrieval, and PostgreSQL reads/writes.

The purple panel describes the implemented Actions delivery sequence. Render and Vercel there refer to the same providers as the runtime panel. The sequential connector expresses workflow order; Render does not call the Vercel CLI itself. GitHub Actions orchestrates both operations. The PR-review step represents the team's required process; this workflow does not enforce teammate approval by itself.

The provider iconography includes Microsoft's official Entra ID architecture icon, PostgreSQL's official elephant logo and Vercel's documented triangle symbol, with product names alongside. Components and directed connectors use conventional deployment/data-flow notation. The diagrams.net document is the editable source; its portable SVG and PNG were generated from the same layout and the PNG was visually checked.

## Evidence and limits

Source implementation: application commit [`9af47f9`](https://github.com/OliviaY1/randomizer/tree/9af47f9c68a07f9ac35bb981877979d305e2ef76). The existing HTTPS endpoints and health response were checked during the audit. Provider credentials, private dashboards, network isolation, database TLS settings, service plans and availability guarantees were not inspected.

The control-plane status reflects the [first main Actions run](https://github.com/OliviaY1/randomizer/actions/runs/37716917941): validation passed, but the production job stopped because the five configuration values were absent. Update the status annotation after a successful production run.

**Questions for teammate review:** Which provider hosts PostgreSQL? What database transport/network controls are actually configured? Are the diagram's component boundaries and Microsoft account scope accurate? Have the real sign-in and saved-state journeys passed? Do not replace these unknowns with an assumed provider or claim of private networking/high availability.

The future-work band contains proposals, not approved commitments or already deployed infrastructure. The current ID-token authorization approach is represented honestly and remains a separate technical follow-up.

## Icon sources

- [Microsoft Entra architecture icons and permitted diagram use](https://learn.microsoft.com/en-us/entra/architecture/architecture-icons). Used the unmodified `Microsoft Entra ID color icon.svg` from the October 2023 official package.
- [PostgreSQL official elephant logo](https://www.postgresql.org/media/img/about/press/elephant.png).
- [Vercel brand guidance and triangle symbol](https://vercel.com/geist/brands).

No assumptions about AWS, Azure hosting, Kubernetes, multiple regions or private subnets have been added to this logical view.
