# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1] - 2024-04-29

### Fixed

- Correctly set `fetch-depth: 0` when `inputs.package-type == 'git'`. This is required to fetch all tags from the git.

## [1.1.0] - 2024-04-29

### Added

- Support for `package-type: git`. This new package type identifies the git source tree as the package registry.
Semver git tags are used to identify package versions. This package type is useful for GitHub Actions and Workflows,
for example.

## [1.0.1] - 2024-04-29

### Fixed

- The usage section of the readme. It was using legacy versions and the wrong workflow name to reference itself.

## [1.0.0] - 2024-04-20

### Added

- First release!

[1.1.1]: https://github.com/infra-blocks/check-has-changelog-version-matching-semver-increment-workflow/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/infra-blocks/check-has-changelog-version-matching-semver-increment-workflow/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/infra-blocks/check-has-changelog-version-matching-semver-increment-workflow/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/infra-blocks/check-has-changelog-version-matching-semver-increment-workflow/releases/tag/v1.0.0
