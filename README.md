# DevsBC/ci

Shared GitHub Actions for **DevsBC** repos on GCP (`satoru-co`).

## Why this repo exists

WIF provider, project number, and deploy service account are **org-wide constants**. They live here once so app repos do not copy the same `env:` block into every workflow.

## GCP defaults (single source of truth)

| Constant | Value |
|----------|--------|
| Project ID | `satoru-co` |
| WIF provider | `projects/551149990002/locations/global/workloadIdentityPools/github/providers/github` |
| Deploy SA | `github-actions-deploy@satoru-co.iam.gserviceaccount.com` |

## Composite action: `gcp-wif-auth`

```yaml
permissions:
  contents: read
  id-token: write

steps:
  - uses: DevsBC/ci/.github/actions/gcp-wif-auth@main
    # optional: with project_id / create_credentials_file / setup_gcloud
```

For Firebase CLI, set `create_credentials_file: true` and use `GOOGLE_APPLICATION_CREDENTIALS` from the auth step outputs (see `itsbiblical` deploy workflow).

## Workflows in this repo

| Workflow | Purpose |
|----------|---------|
| `wif-smoke.yml` | Manual check that WIF + gcloud work from GitHub |

App-specific build/deploy (Cloud Run matrix, Angular, etc.) stays in each application repository.
