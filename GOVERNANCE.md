# Governance

This document describes how the OpenDecree project is governed: who maintains it, how contributors can grow their involvement, and how significant decisions are made.

OpenDecree is an alpha-stage open-source project. Governance will evolve as the community grows.

## Roles

### Maintainer

Maintainers have write access to the repositories and are responsible for the day-to-day health of the project.

**Responsibilities:**
- Review and merge pull requests
- Triage issues and manage the project board
- Facilitate the decision-making process for significant changes
- Own the release process (see [Release process](#release-process) below)
- Uphold the [Code of Conduct](CODE_OF_CONDUCT.md)

**How to become a maintainer:**
There is no formal process during alpha. Active contributors who demonstrate sustained, high-quality involvement may be invited by an existing maintainer. The bar is consistent contribution over time, good judgment, and familiarity with the project's goals and conventions.

### Contributor

Anyone who opens an issue, submits a pull request, or participates in discussions is a contributor.

**How to contribute:**
- Open an issue to report a bug or propose a feature
- Submit a pull request — all PRs are reviewed by a maintainer
- Join the conversation in [Discussions](https://github.com/orgs/opendecree/discussions)

There is no formal contributor list or recognition tier during alpha.

## Decision-making

### Day-to-day changes

Small changes (bug fixes, documentation, minor features, dependency bumps) are decided by the reviewing maintainer. A single approval is sufficient to merge.

### Significant changes

Significant changes include:

- New top-level features or subsystems
- Breaking changes to the API, proto definitions, or SDK interfaces
- Changes to project conventions, tooling, or repository structure
- Deprecations

For these, the proposer opens a **design issue** (labeled `design`) describing the problem, the proposed approach, and alternatives considered. Maintainers discuss in the issue and reach a decision by lazy consensus: if no maintainer objects within a reasonable time (typically one week), the proposal is accepted. If there is disagreement, maintainers work toward a decision through discussion; in the case of deadlock, the lead maintainer has the final say.

### Breaking changes

During alpha, breaking changes are made freely. The project does not maintain backward compatibility before v1.0. Breaking changes must be noted in the pull request and reflected in the changelog.

## Release process

Releases are owned by maintainers. The release conventions — versioning, tag format, release order across repos, and per-repo details — are documented in [RELEASING.md](RELEASING.md).

At a high level:
- Releases follow [Semantic Versioning](https://semver.org/)
- Each repository versions independently
- The core service (`decree`) is released first; SDKs follow
- Release artifacts are published automatically by CI on tag push

## Code of conduct

All participants are expected to follow the [Code of Conduct](CODE_OF_CONDUCT.md).
