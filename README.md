# labeler-action

This action is compatible with [actions/labeler](https://github.com/actions/labeler) and can use existing label definition files (such as `labeler.yml`) without modification.

## Features

- **Compatible with actions/labeler**: You can use your existing label definition files without any changes.
- **Flexible triggers**: Can be executed on events other than `on: pull_request` (e.g., `push`, `workflow_dispatch`, etc.), allowing labeling processes outside of pull request events.
- **Supports label color settings**

## Usage

- Prepare a label definition file (e.g., `.github/labeler.yml`):

```yaml
ci:
  - any:
    - changed-files:
      - any-glob-to-all-files: '.github/**'
      - all-files-to-any-glob:
        - '*.yml'
        - '.github/**'
  - color: '#7c0bb2'
  - codeowners:
    - srz-zumix
    - "@org/team"
    - "org/team..." # expand team members
  - author: '@myorg/developers'
  # - all-files-to-any-glob:
  #   - '*.yml'
  #   - '.github/**'

bot:
  - author: '.*\[bot\]$'
```

> **Note:**  
> The `color`, `description`, `codeowners`, and `author` settings are ignored by `actions/labeler`. It will not cause an error.  
> The `all-files-to-any-glob` under `changed-files` is not compatible with `actions/labeler`. If you want a configuration file that can be loaded by `actions/labeler`, write `all-files-to-any-glob` at the same level as `color`.  
> If you want to expand teams, you need a token with read access to team members.

- Call this action in your workflow:

```yaml
name: Labeler
on:
  pull_request:
    types:
      - opened
      - synchronize
      - reopened
      - ready_for_review # for review-request: ready_for_review/always_reviewable
  workflow_dispatch:
    inputs:
      pr-number:
        description: 'The pull request number(s)'
        required: true      

jobs:
  labeler:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      issues: write
      pull-requests: write
    steps:
      - uses: srz-zumix/labeler-action@v0
        with:
          pr-number: ${{ github.event.inputs.pr-number || github.event.number }}
          review-request: ready_for_review
```

## Environments

| Name | Description |
|------|-------------|
| GH_HOST | Specify the GitHub hostname |
| GH_TOKEN | Takes priority over inputs.repo-token |
| GH_ENTERPRISE_TOKEN | Takes priority over inputs.repo-token |
| AQUA_GITHUB_TOKEN | Set AQUA_GITHUB_TOKEN to avoid rate limits |

## Reference

- [actions/labeler](https://github.com/actions/labeler)
- [srz-zumix/gh-label-kit](https://github.com/srz-zumix/gh-label-kit)
- [aquaproj/aqua](https://github.com/aquaproj/aqua)
- [srz-zumix/aqua-installer-cache](https://github.com/srz-zumix/aqua-installer-cache)
