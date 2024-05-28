# sync-upstream-release-tag

Action to automatically create tag in a forked repo for the latest upstream release.

It relies on the presence of a `last-dd-base` tag in the branch to find your latest commit and rebase them on top of the latest commits from upstream. It tags the new branch with a new tag according to the schema below:
![sync workflow](./sync%20workflow.png)

## Usage

1. Add a secret called `WORKFLOW_TOKEN` with write access to your repository
2. Add this to a `.github/workflows/sync.yaml` file in your fork

```yaml
name: Sync upstream
# This runs every day on 1000 UTC
on:
  schedule:
    - cron: '0 10 * * *'
  # Allows manual workflow run
  workflow_dispatch:

permissions: write-all

jobs:
 build:
  runs-on: ubuntu-latest
  steps:
    - name: Create upstream version tag
      uses: DataDog/sync-upstream-release-tag@v0.1.0
        with:
          github_actor: "${GITHUB_ACTOR}"
          github_repository: "${GITHUB_REPOSITORY}"
          # The token need to have write-all access on the repo for the rebase
          # so we cannot use the action's default token
          github_token: ${{ secrets.WORKFLOW_TOKEN }}
          upstream_repo: kubernetes-sigs/gcp-compute-persistent-disk-csi-driver
```
