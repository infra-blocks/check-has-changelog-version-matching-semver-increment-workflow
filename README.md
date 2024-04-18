# check-has-changelog-version-matching-semver-label-workflow
[![Git Tag Semver From Label](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/git-tag-semver-from-label.yml/badge.svg)](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/git-tag-semver-from-label.yml)
[![Update From Template](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/actions/workflows/update-from-template.yml)

This workflow enforces the presence of a changelog entry matching the version that would be released based on the PR
label. See [check-has-semver-label-workflow]().

For example, if the current version is `1.2.3` and the PR has a `minor` label associated, the workflow will check that
the changelog contains an entry for `1.3.0`. It will fail otherwise.

When the label is "no version", this workflow is a big no-op. *Do not skip the workflow* with an `if` expression based
on that label (`if: steps.semver-label.outputs.matched-label != 'no version'`), as GitHub Actions has an issue where
the path of a skipped required check is not the same as the path of an actual check.

## Inputs

|     Name      | Required | Description       |
|:-------------:|:--------:|-------------------|
| example-input |   true   | An example input. |

## Secrets

|      Name      | Required | Description        |
|:--------------:|:--------:|--------------------|
| example-secret |   true   | An example secret. |

## Outputs

|      Name      | Description        |
|:--------------:|--------------------|
| example-output | An example output. |

## Permissions

|     Scope     | Level | Reason   |
|:-------------:|:-----:|----------|
| pull-requests | read  | Because. |

## Concurrency controls

Describe concurrency controls of the workflow.

## Timeouts

Describe the timeouts configured, if any.

## Usage

```yaml
name: Template Usage

on:
  push: ~

# This needs to be a superset of what your workflow requires
permissions:
  pull-requests: read

jobs:
  example-job:
    uses: infrastructure-blocks/check-has-changelog-version-matching-semver-label-workflow/.github/workflows/workflow.yml@v1
    with:
      example-input: Nobody cares
    secrets:
      example-secret: ${{ secrets.EXAMPLE }}
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
