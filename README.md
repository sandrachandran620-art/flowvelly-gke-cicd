# Flowvelly GKE Inference Service CI/CD

This repository provides the deployment pipeline and infrastructure configuration for Flowvelly Healthcare's containerized inference service on Google Kubernetes Engine (GKE).

## Directory Structure
```text
flowvelly-gke-cicd/
├── .github/
│   └── workflows/
│       ├── deploy.yml         # CI/CD deployment pipeline (Stages 1-5)
│       └── rollback.yml       # Manual on-demand emergency rollback
├── manifests/
│   └── inference-service.yaml # GKE deployment and service resources
├── README.md                  # Project overview, assumptions, and navigation guide
├── TROUBLESHOOTING.md         # OIDC and principal mapping resolution
├── RUNBOOK.md                 # Gate approvals and rollback execution
└── AI_COMPANION_NOTE.md       # AI utilization critique and gaps corrected


# Architectural Assumptions

- GKE clusters named `qa-cluster`, `preprod-cluster`, and `prod-cluster` already exist in the target GCP project.
- Each environment uses its own Kubernetes namespace (`qa`, `preprod`, `prod`); the pipeline creates these if they don't already exist.
- An Artifact Registry repository named `flowvelly-repo` already exists in the specified region.
- The container image exposes a `/healthz` endpoint on port 8080 for liveness and readiness checks.
- The GCP service account, Workload Identity Pool, and Workload Identity Provider (as described in TROUBLESHOOTING.md) are already configured before this pipeline runs.
- GitHub Environments `preprod` and `prod` have required reviewers configured to enforce the approval gate.

## Future Scoping

- Reintroduce automated Infrastructure-as-Code scanning (e.g. Checkov) once the Terraform module is placed in this same repository.
- Add automated integration or smoke tests as a required check before promoting from qa.
- Add pipeline notifications (Slack or email) when a deployment is waiting on approval.
- Add CMEK (customer-managed encryption keys) for storage and Artifact Registry as a further hardening step.
- Evaluate a lighter-weight approval path for rollback specifically, separate from the standard deploy gate, to reduce recovery time during an active incident.

