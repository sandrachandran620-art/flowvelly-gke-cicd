# AI Companion Review Note

### Usage Strategy
AI was utilized to draft GKE Deployment Manifests and GitHub Actions templates to accelerate file composition.

### Shortfalls Corrected
1.  The raw AI-generated templates ran the image scan in a separate job without passing authentication credentials. On private Artifact Registries, this caused scan jobs to fail with unauthorized pull errors. We fixed this by adding explicit GCP OIDC authentication to the scan job.
2.  The AI workflows referenced `needs.build-and-push.outputs.image_tag` in the `deploy-preprod` and `deploy-prod` jobs, but did not declare `build-and-push` as a dependency in their `needs` list. This resulted in empty image tag resolutions. We fixed this by adding `build-and-push` to the execution dependencies list.
3.  The AI recommended a raw `kubectl set image` command which fails when deploying to fresh namespaces where the deployment resources do not yet exist. We added namespace creation and manifest initialization steps to ensure deployment success on new namespaces.

### Rollback Environment Gate Trade-off (Human Architectural Decision)
Our manual rollback workflow (rollback.yml) deliberately reuses the same GitHub Environment security configuration for prod and preprod as the main deployment pipeline.

* **The Security Trade-off:** In an active production incident, requiring a manual approval gate slightly delays rollout recovery (the "fast rollback" goal) because an authorized gatekeeper must still click approve on GitHub before the job executes.
* **The Compliance Justification:** In a regulated healthcare environment handling PHI-bordering data, permitting automated, un-audited state modifications on production GKE clusters is a compliance risk. By routing rollbacks through the exact same environment gate, we enforce strict continuous change control, leaving a permanent audit trail showing who approved the rollback, what was reverted, and when the action occurred.
