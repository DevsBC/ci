# DevsBC/ci

Shared **public** GitHub Actions for **DevsBC** repos on GCP (satoru-co).

> This repo must stay **public**. Private actions cannot be `uses:` from other repos on GitHub Free — Actions reports `repository not found` even if you own both.

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
  - id: gcp
    uses: DevsBC/ci/.github/actions/gcp-wif-auth@main

  - uses: google-github-actions/setup-gcloud@v2

  # gcloud / docker / firebase steps — set ADC when a tool needs the file explicitly:
  env:
    GOOGLE_APPLICATION_CREDENTIALS: ${{ steps.gcp.outputs.credentials_file_path }}
```

Always run `setup-gcloud` **after** this action in the **job** (not inside another composite). The action only performs WIF auth and leaves a credentials file for the rest of the job.

## Workflows in this repo

| Workflow | Purpose |
|----------|---------|
| `wif-smoke.yml` | Manual check that WIF + gcloud work from GitHub |

App-specific build/deploy stays in each application repository.
