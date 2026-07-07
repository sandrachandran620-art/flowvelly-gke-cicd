# AI Companion Note

### 1. How AI Was Utilized
AI assistants were leveraged to draft initial GKE Deployment Manifests, Terraform skeleton patterns, and boilerplate GitHub Actions workflow templates. This allowed us to focus effort on policy verification, secure IAM integrations, and change management procedures.

### 2. Shortfalls Corrected
*  The raw AI-generated templates assumed open internet egress for container building and standard, permanent long-lived `.json` service account credentials. In a locked-down, HIPAA-compliant healthcare estate, open egress is highly restricted, and static private keys represent a major audit risk. I corrected this by implementing Workload Identity Federation (WIF) OIDC token exchanges instead of static keys.
*  The AI tool suggested leaving the OIDC Attribute Condition text area blank for "easy debugging." I corrected this shortfall after encountering backend validation errors, enforcing structured Common Expression Language (CEL) checks (`assertion.repository.startsWith("sandrachandran620-art/")`) to guarantee that only validated GitHub pipelines can assume our service account.
*  The AI suggested using the Cloud Console UI to bind the `Subject` to `*`. I caught the console-generated IAM path violation (`/subject/*`) and manually resolved it via the `gcloud` CLI using a valid global resource path pattern.

### 3. Human Value Add (What the AI Missed)
The AI missed the strict organizational and security policies required for a protected healthcare environment. I added:
*  Stripped away generic broad owner/editor permissions and restricted the pipeline service account to GKE Developer, Artifact Registry Writer, and Cloud SQL Client roles.
*  Configured mandatory environments (`preprod` / `prod`) requiring human validation before any high-environment rollout is triggered.
