# actions-workflows
Repository of plus3it reusable worfklows for GitHub Actions.

This project publishes reusable workflows for the Plus3IT organization. All reusable
workflows are located in the directory [.github/workflows](.github/workflows).

* [lint](.github/workflows/lint.yml)
* [test](.github/workflows/test.yml)
* [test-terrafirm-integration](.github/workflows/test-terrafirm-integration.yml)
* [release](.github/workflows/release.yml)

The release logic is also available as a composite action. Only difference is that
the action does not execute the lint and test workflows.

* [release action](.github/actions/release)

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

```yaml
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
* `prerelease`: Marks the GitHub Release as a prerelease. Defaults to `false`.
* `draft`: Creates the GitHub Release as a draft. Defaults to `false`.

An example of calling the reusable `test` workflow:

```yaml
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

### `test-terrafirm-integration`

Reusable workflow for running terrafirm integration tests in salt formula repositories.
The caller workflow is responsible for defining trigger conditions (e.g., PR review comment,
workflow dispatch, schedule) and specifying the test matrix. This workflow handles test
execution with CodeBuild runners for a single source build.

Inputs:

* `source-build`: **Required.** Source build to test (e.g., al2023, centos9stream, rhel8, rhel9, ol9).
* `watchmaker-git-url`: Watchmaker Git clone URL. Defaults to `https://github.com/plus3it/watchmaker.git`.
* `watchmaker-git-ref`: Watchmaker Git ref (branch/tag). Defaults to `main`.
* `watchmaker-common-args`: Watchmaker arguments passed to terrafirm, shared by linux and windows platforms. Defaults to `"-n -e dev"`.
* `formula-archive-url`: Optional URL for formula zip archive. If omitted, dynamically generated from event.

An example of calling the reusable `test-terrafirm-integration` workflow:

```yaml
name: Run terrafirm integration tests

on:
  workflow_dispatch:
    inputs:
      watchmaker-git-url:
        description: Watchmaker git clone URL
        required: false
        default: https://github.com/plus3it/watchmaker.git
        type: string
      watchmaker-git-ref:
        description: Watchmaker git ref
        required: false
        default: main
        type: string
      formula-archive-url:
        description: Optional URL to the salt formula zip archive
        required: false
        default: ""
        type: string

  pull_request_review:
    types: [submitted]

permissions:
  contents: read

jobs:
  integration:
    if: contains(github.event.review.body, '/build') || github.event_name == 'workflow_dispatch'
    strategy:
      fail-fast: false
      matrix:
        source-build:
          - al2023
          - centos9stream
          - rhel8
          - rhel9
          - ol9
    uses: plus3it/actions-workflows/.github/workflows/test-terrafirm-integration.yml@<ref>
    with:
      source-build: ${{ matrix.source-build }}
      watchmaker-git-url: ${{ github.event.inputs.watchmaker-git-url }}
      watchmaker-git-ref: ${{ github.event.inputs.watchmaker-git-ref }}
      formula-archive-url: ${{ github.event.inputs.formula-archive-url }}
```

### `release`

Inputs:

* `mockstacktest-enable`: Controls whether to run the mockstacktest job. Defaults
  to `true`.

Secrets:
  * `release-token`: Required. Token with permissions to create GitHub Releases.

An example of calling the reusable `release` workflow:

```yaml
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

An example of using the composite `release` action:

Inputs:

* `release-files`: Optional. Newline-delimited globs of assets to upload.
* `prerelease`: Optional. Marks the GitHub Release as a prerelease. Defaults to `false`.
* `draft`: Optional. Creates the GitHub Release as a draft. Defaults to `false`.
* `release-token`: Required. Token with permissions to create GitHub Releases.

```yaml
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
    runs-on: ubuntu-latest
    steps:
      - uses: plus3it/actions-workflows/.github/actions/release@v1
        with:
          release-token: ${{ secrets.GH_RELEASES_TOKEN }}
```
