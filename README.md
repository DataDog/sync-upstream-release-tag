# sync-upstream-release-tag
Action to automatically create tag in a forked repo for the latest upstream release

# Usage
Add this to a `.github/workflows/sync.yaml` file in your fork
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
	  github_token: ${{ secrets.GITHUB_TOKEN }}
          upstream_repo: kubernetes-sigs/gcp-compute-persistent-disk-csi-driver
```
