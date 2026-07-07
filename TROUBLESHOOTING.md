# Troubleshooting Write-up: GKE OIDC Setup Friction

During the establishment of Workload Identity Federation (WIF) for the Flowvelly Healthcare GKE deployment, we analyzed and resolved critical friction points related to OIDC identity provider validation in enterprise GCP environments.

### Case 1: The Attribute Condition CEL Validation Error
* **Symptom:** Saving the provider threw `INVALID_ARGUMENT: The attribute condition must reference one of the provider's claims.`
* **Root Cause Analysis:** In modern GCP environments, Google Cloud enforces an attribute condition for GitHub Actions OIDC setups by design. Attempting to leave the "Attribute Condition" text box empty or mismatched results in a generic backend failure.
* **Resolution:** Mapped `google.subject` to `assertion.sub` and `attribute.repository` to `assertion.repository` correctly, then defined a strict CEL restriction matching our organizational pattern: `assertion.repository.startsWith("sandrachandran620-art/")`.

### Case 2: Misleading Cloud Console Wildcard Formatting
* **Symptom:** Saving GKE principal mappings via the UI threw `INVALID_ARGUMENT: The member principalSet://.../subject/* is of an unknown type.`
* **Root Cause Analysis:** The Cloud Console interface incorrectly formats the subject wildcard mapping as `/subject/*`, which is rejected by Google Cloud's IAM engine.
* **Resolution:** Bypassed the console UI bug by binding the IAM policies directly via the Google Cloud CLI, using the valid global wildcard path format: `principalSet://iam.googleapis.com/projects/YOUR_PROJECT_NUM/locations/global/workloadIdentityPools/YOUR_POOL/*`.
