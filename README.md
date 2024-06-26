# sync-upstream-release-tag

Action to automatically create tag in a forked repo for the latest upstream release.

It relies on the presence of a `last-dd-base` tag in the branch to find your latest commit and rebase them on top of the latest commits from upstream. It tags the new branch with a new tag according to the schema below:
![sync workflow](./sync%20workflow.png)

## Usage

1. Add a secret called `WORKFLOW_TOKEN` with write access to your repository. This must be a [personal github access token](https://github.com/settings/tokens) with at least the `repo` and `workflow` permissions (the scope to update the Github Actions is needed in case the upstream repo modifies its own actions):
   ![token_scope](./token%20scope.png)
2. If your org enforces the use of SSO, you must [authorize your token](https://docs.github.com/en/enterprise-cloud@latest/authentication/authenticating-with-saml-single-sign-on/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on)
3. Add this workflow definition to a `.github/workflows/sync.yaml` file in your fork:

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

Additional configuration flags are available if needed. Check the [action](./action.yml) directly to see them.
