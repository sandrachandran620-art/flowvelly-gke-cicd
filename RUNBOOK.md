# Flowvelly GKE Inference Service Operations Runbook

### Standard Deployment Gate Procedure
1. Code commits or merges to `main` trigger the CI pipeline.
2. The code builds, undergoes vulnerability scanning (Trivy), and passes IaC policy analysis (Checkov).
3. The workload automatically deploys to the `qa` namespace.
4. Deployment to `preprod` and `prod` halts. Go to **GitHub Actions** -> Select the active workflow run -> Click **Review deployments** -> Approve the release.

### Emergency Rollback Runbook (Under 2 Seconds)
In the event of an active production incident:
1. Navigate to the **Actions** tab of the `flowvelly-gke-cicd` repository.
2. In the left-hand sidebar, select **Emergency On-Demand Rollback**.
3. Click the **Run workflow** dropdown on the right.
4. Under **Target environment to roll back**, select `prod` (or `preprod`).
5. Click **Run workflow**. This immediately triggers a direct `kubectl rollout undo` on the GKE cluster, reverting the inference service image to its last-known stable revision instantly.
