# GitHub <-> Azure DevOps Repo Sync

This repository contains a GitHub Actions workflow that synchronizes:

- Branches
- Tags
- Commits (through branch and tag ref updates)

Workflow file:

- `.github/workflows/cd.yaml`

## Supported directions

- GitHub -> Azure DevOps
  - Automatic on every push and branch deletion event.
- Azure DevOps -> GitHub
  - Optional, runs on schedule (every 30 minutes) and manual dispatch.

## Required GitHub repository secrets

1. `ADO_REMOTE_URL`
   - Full Azure DevOps repo URL without credentials.
   - Example:
     - `https://dev.azure.com/my-org/my-project/_git/my-repo`

2. `ADO_PAT`
   - Azure DevOps PAT with code read and write permissions for the target repo.

3. `GH_PUSH_TOKEN` (required only for Azure DevOps -> GitHub sync)
   - GitHub token with `contents:write` permission on this repo.

## Manual run

From Actions, run workflow and choose input `direction`:

- `github-to-ado`
- `ado-to-github`
- `both`

## Notes and operational guidance

- Branch and tag deletions are propagated by prune behavior on push.
- If branch protection blocks action pushes, use a dedicated technical account token in `GH_PUSH_TOKEN` and configure branch protection exceptions.
- The workflow uses force updates for tags to keep both sides consistent.
- Bidirectional sync can create frequent no-op runs; this is expected and safe.
