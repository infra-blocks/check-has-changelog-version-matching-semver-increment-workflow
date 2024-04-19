# check-has-changelog-version-matching-semver-label-workflow
[![Git Tag Semver From Label](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/git-tag-semver-from-label.yml/badge.svg)](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/git-tag-semver-from-label.yml)
[![Update From Template](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/update-from-template.yml)

This workflow enforces the presence of a changelog entry matching the version that would be released based on the PR
label. It is meant to be used in conjunction with [check-has-semver-label-workflow](https://github.com/infrastructure-blocks/check-has-semver-label-workflow).

For example, if the current version is `1.2.3` and the PR has a `minor` label associated, the workflow will check that
the changelog contains an entry for `1.3.0`. It will fail otherwise.

When the label is "no version", this workflow is a big no-op. *Do not skip the workflow* with an `if` expression based
on that label (`if: steps.semver-label.outputs.matched-label != 'no version'`), as GitHub Actions has an issue where
the path of a skipped required check is not the same as the path of an actual check.

## Inputs

|      Name      | Required | Description                                                                                                                                                                              |
|:--------------:|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| changelog-file |  false   | The changelog file. See [check-has-changelog-version-action](https://github.com/infrastructure-blocks/check-has-changelog-version-action).                                               |
|     label      |   true   | The semver label that indicates which release version will be produced. See [check-has-semver-label-workflow](https://github.com/infrastructure-blocks/check-has-semver-label-workflow). |
|  package-file  |  false   | The package file from which to extract the current version. See [package-version-action](https://github.com/infrastructure-blocks/package-version-action).                               |
|  package-type  |   true   | The type of package produced by the repository. [package-version-action](https://github.com/infrastructure-blocks/package-version-action).                                               |
|      skip      |  false   | A boolean indicating whether to skip the workflow. This is to workaround the required checks discrepancy when the workflow is skipped from the caller. It defaults to false.             |

## Secrets

N/A

## Outputs

N/A

## Permissions

|     Scope     | Level | Reason                         |
|:-------------:|:-----:|--------------------------------|
| pull-requests | write | Needed to post status updates. |

## Concurrency controls

N/A

## Timeouts

N/A

## Usage

```yaml
name: PR Checks

on:
  pull_request:
    types:
      - opened
      - reopened
      - synchronize
      - labeled
      - unlabeled

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  check-has-semver-label:
    permissions:
      pull-requests: write
    uses: infrastructure-blocks/check-has-semver-label-workflow/.github/workflows/workflow.yml@v1
  check-has-changelog-version-matching-semver-label:
    needs: 
      - check-has-semver-label
    permissions:
      contents: read
      pull-requests: write
    uses: infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/.github/workflows/workflow.yml@v1
    with:
      package-type: npm
      label: ${{ needs.check-has-semver-label.outputs.matched-label }}
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
