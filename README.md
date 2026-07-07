# Flowvelly Healthcare GKE CI/CD Pipeline

This repository contains the production-grade, secure continuous integration and delivery (CI/CD) pipeline for Flowvelly Healthcare's containerized inference service workloads on Google Kubernetes Engine (GKE).

## 1. Directory Structure & Navigation

This repository is organized to maintain a clean separation of concerns between application delivery pipeline configurations, Kubernetes manifests, and system documentation:

```text
flowvelly-gke-cicd/
├── .github/
│   └── workflows/
│       ├── deploy.yml         # CI/CD deployment pipeline (Stages 1-6)
│       └── rollback.yml       # Manual on-demand emergency rollback
├── manifests/
│   └── inference-service.yaml # Kubernetes Deployment & Service Manifest
├── README.md                  # Project overview, assumptions, and navigation guide
├── TROUBLESHOOTING.md         # OIDC setup friction and platform troubleshooting write-up
├── RUNBOOK.md                 # System release procedures and emergency rollback operations
└── AI_COMPANION_NOTE.md       # AI utilization audit, shortfalls corrected, and human additions
