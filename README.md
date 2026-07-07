# Flowvelly GKE Inference Service CI/CD

This repository contains the application deployment pipeline and Kubernetes resources for Flowvelly Healthcare's containerized inference service on Google Kubernetes Engine (GKE).

---

## 1. Directory Structure & Navigation

This repository separates pipeline automations, target cluster configurations, and setup documentation:

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
