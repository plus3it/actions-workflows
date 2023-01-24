# actions-workflows
Repository of plus3it reusable worfklows for GitHub Actions.

This project publishes reusable workflows for the Plus3IT organization. All reusable
workflows are located in the directory [.github/workflows](.github/workflows).

* [lint](.github/workflows/lint.yml)
* [test](.github/workflows/test.yml)
* [release](.github/workflows/release.yml)

Any workflow file that is not prefixed with `local-` is provided as a reusable
workflow. The `local-` workflow files are the workflows in use by _this_ project,
themselves using the reusable workflows. The `local-` workflows are also examples
of how the reusable workflows are expected to be invoked.

## Reusable workflows

### `lint`

Inputs:

* `tardigradelint-target`: Controls which tardigrade-ci Makefile target to run.
  Defaults to `lint`.

An example of calling the reusable `lint` workflow:

```
name: Run lint and static analyis checks
on:
  pull_request:

# Cancel other lint workflows when source is updated
concurrency:
  group: lint-${{ github.head_ref || github.ref }}
  cancel-in-progress: true

jobs:
  lint:
    uses: plus3it/actions-workflows/.github/workflows/lint.yml@v1
```

### `test`

Inputs:

* `mockstacktest-enable`: Controls whether to run the mockstacktest job. Defaults
  to `true`.

An example of calling the reusable `test` workflow:

```
name: Run test jobs
on:
  pull_request:

# Cancel other test workflows when source is updated
concurrency:
  group: test-${{ github.head_ref || github.ref }}
  cancel-in-progress: true

jobs:
  test:
    uses: plus3it/actions-workflows/.github/workflows/test.yml@v1
```

### `release`

Inputs:

* `mockstacktest-enable`: Controls whether to run the mockstacktest job. Defaults
  to `true`.

Secrets:
  * `release-token`: Required. Token with permissions to create GitHub Releases.

An example of calling the reusable `release` workflow:

```
name: Create GitHub Release

on:
  # Run on demand
  workflow_dispatch:

  # Run on push to main when .bumpversion.cfg version is updated
  push:
    branches:
      - main
    paths:
      - .bumpversion.cfg

jobs:
  release:
    uses: plus3it/actions-workflows/.github/workflows/release.yml@v1
    secrets:
      release-token: ${{ secrets.GH_RELEASES_TOKEN }}
```
