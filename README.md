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
- color: '#7c0bb2'
```

> **Note:** The `color` setting is ignored by `actions/labeler`. It will not cause an error.

- Call this action in your workflow:

```yaml
name: Labeler
on:
  pull_request:
  workflow_dispatch:
    inputs:
      pr-number:
        description: 'The pull request number(s)'
        required: true      

jobs:
  labeler:
    runs-on: ubuntu-latest
    permissions:
      issues: write
      pull-requests: write
    steps:
      - uses: srz-zumix/labeler-action@v0
        with:
          pr-number: ${{ github.event.inputs.pr-number || github.event.number }}
```

## Reference

- [actions/labeler](https://github.com/actions/labeler)
- [srz-zumix/gh-label-kit](https://github.com/srz-zumix/gh-label-kit)
