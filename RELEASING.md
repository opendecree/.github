# Release Conventions

Cross-repo release conventions for the OpenDecree project. Each repo has its own release mechanism — this document defines the shared conventions and per-repo specifics.

## General Rules

- **SemVer** — all repos follow [Semantic Versioning](https://semver.org/)
- **Independent versions** — repos version independently (decree v0.6.0, Python SDK v0.2.0, etc.)
- **Alpha status** — all repos are alpha; breaking changes happen freely
- **Tag format** — `v{MAJOR}.{MINOR}.{PATCH}` with optional pre-release suffix
- **Release order** — decree first (API/proto changes), then SDKs, then demos
- **Never force-push tags** — if a tag is wrong, use the next version number

## Pre-Release Tags

Pre-release format differs by ecosystem:

| Repo | Tag | Package Version | Example |
|------|-----|-----------------|---------|
| decree (Go) | `v0.6.0-alpha.1` | same | `go get ...@v0.6.0-alpha.1` |
| decree-python | `v0.2.0a1` | `0.2.0a1` | PEP 440: `a` = alpha, `b` = beta, `rc` = release candidate |
| decree-typescript | `v0.2.0-alpha.1` | `0.2.0-alpha.1` | npm semver pre-release |
| decree-ui | `v0.1.0-alpha.1` | N/A (Docker only) | GHCR tag |

**Why different?** Each package registry has its own pre-release convention. Go and npm use `-alpha.N`, Python PEP 440 uses `aN`. The git tag must produce a valid version for the target registry.

## Per-Repo Details

### decree (Go monorepo)

**Versioning:** Go multi-module — each module gets its own tag at release time.

**Tags per release (8):**
```
v{X.Y.Z}
api/v{X.Y.Z}
cmd/decree/v{X.Y.Z}
sdk/configclient/v{X.Y.Z}
sdk/adminclient/v{X.Y.Z}
sdk/configwatcher/v{X.Y.Z}
sdk/grpctransport/v{X.Y.Z}
sdk/tools/v{X.Y.Z}
```

> **GitHub platform limit — push tags individually.** GitHub silently drops
> **all** tag push events when more than three tags are pushed at once
> ([docs](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)).
> With eight tags per decree release, `git push origin --tags` results in zero
> workflow triggers. The `/release` skill pushes each tag on its own line to
> stay under the limit; do not replace that with `--tags` or a bulk push.

**Trigger:** Push of `v*` tag triggers `release.yml`.

**Artifacts:**
- GitHub Release with auto-generated notes
- GoReleaser binaries (server + CLI, linux/darwin, amd64/arm64)
- Docker images: `ghcr.io/opendecree/decree:{version}` + `:latest`
- Docker CLI image: `ghcr.io/opendecree/decree-cli:{version}` + `:latest`
- BSR push: `buf.build/opendecree/decree`

**Version source:** Git tags (Go modules resolve versions from tags).

**Pre-release:** GoReleaser `prerelease: auto` detects `-alpha`, `-beta`, `-rc` and marks the GitHub Release accordingly. Docker images still publish with the pre-release tag.

**Checklist:** `docs/development/checklists.md` — "Before Release Tag" and "Release Tag Process" sections.

### decree-python

**Tags per release (1):** `v{X.Y.Z}` or `v{X.Y.Z}aN` (alpha) / `v{X.Y.Z}bN` (beta) / `v{X.Y.Z}rcN` (RC)

**Trigger:** Push of `v*.*.*` tag triggers `publish.yml`.

**Artifacts:**
- PyPI package: `opendecree`
- Trusted publisher (OIDC, no API tokens)

**Version source:** `sdk/pyproject.toml` → `version` field. Must match the tag (without `v` prefix).

**Pre-release steps:**
1. Update `version` in `sdk/pyproject.toml` (e.g., `"0.2.0a1"`)
2. Commit and push
3. Tag: `git tag v0.2.0a1 && git push origin v0.2.0a1`

### decree-typescript

**Tags per release (1):** `v{X.Y.Z}` or `v{X.Y.Z}-alpha.N`

**Trigger:** Push of `v*.*.*` tag triggers `publish.yml`.

**Artifacts:**
- npm package: `@opendecree/sdk`
- Trusted publisher with provenance (OIDC, no API tokens)

**Version source:** `package.json` → `version` field. Must match the tag (without `v` prefix).

**Pre-release steps:**
1. Update `version` in `package.json` (e.g., `"0.2.0-alpha.1"`)
2. Commit and push
3. Tag: `git tag v0.2.0-alpha.1 && git push origin v0.2.0-alpha.1`

### decree-ui

**Tags per release (1):** `v{X.Y.Z}` or `v{X.Y.Z}-alpha.N`

**Trigger:** Not yet configured. Will publish Docker image on tag push.

**Artifacts (planned):**
- Docker image: `ghcr.io/opendecree/decree-ui:{version}` + `:latest`
- No package registry — served via Docker or embedded in decree binary (future)

**Version source:** `package.json` → `version` field (for image metadata).

### demos

**No releases.** The demos repo tracks the latest decree release via `docker-compose.yml` image tags. When decree publishes a new version, demo compose files are updated to reference it.

## Coordinated Releases

When a decree release includes proto/API changes that affect SDKs:

1. **decree** — tag and release (triggers Docker, BSR, binaries)
2. **Regenerate proto stubs** in Python and TypeScript SDKs
3. **decree-python** — update stubs, bump version, tag and release
4. **decree-typescript** — update stubs, bump version, tag and release
5. **demos** — update Docker image tags in compose files

For decree-only changes (no API changes), SDK releases are not needed.

## Compatibility Matrix

Proto stubs are the coupling point. Both SDKs generate stubs from decree's `proto/` directory. The generated stubs must match the server version.

### Proto Compatibility

| SDK Version | Built Against Proto | Compatible Server Versions |
|------------|--------------------|-----------------------------|
| decree-python v0.1.0 | decree v0.5.0 proto | decree ≥ v0.5.0 |
| decree-typescript v0.1.0 | decree v0.5.0 proto | decree ≥ v0.5.0 |
| decree Go SDK v0.5.0 | same repo (api/ module) | decree ≥ v0.5.0 |
| decree Go SDK v0.8.0-alpha.1 | same repo (api/ module) | decree ≥ v0.8.0-alpha.1 |
| decree Go SDK v0.9.0-alpha.1 | same repo (api/ module) | decree ≥ v0.9.0-alpha.1 |
| decree-python v0.2.0a1 | decree v0.8.0-alpha.1 proto | decree ≥ v0.8.0-alpha.1 |
| decree-typescript v0.2.0-alpha.1 | decree v0.8.0-alpha.1 proto | decree ≥ v0.8.0-alpha.1 |

**Rule:** When decree makes a proto change (new field, new RPC, breaking change), SDKs must regenerate stubs and release. Non-proto decree changes don't require SDK releases.

### Runtime Requirements

| Component | Requirement |
|-----------|-------------|
| decree server | PostgreSQL 17+, Redis 7+ (or `STORAGE_BACKEND=memory`) |
| decree CLI | None (static binary) |
| Go SDK | Go 1.24+ |
| Python SDK | Python 3.11+ |
| TypeScript SDK | Node.js 20+ |
| Admin GUI | Modern browser (Chrome/Firefox/Safari/Edge) |

### Stub Generation

SDKs generate proto stubs from a local checkout of the decree repo:

| SDK | Generator | Source | Output |
|-----|-----------|--------|--------|
| Go | buf (in Docker) | `proto/` | `api/centralconfig/v1/*.pb.go` (committed) |
| Python | grpcio-tools (in Docker) | `../decree/proto` mounted | `sdk/src/opendecree/_generated/` (committed) |
| TypeScript | ts-proto via buf | `../decree/proto` mounted | `src/generated/` (committed) |

All generated stubs are committed to git — builds work without proto tooling. Regenerate only when proto changes.

### decree-ui OpenAPI Spec

The Admin GUI uses an OpenAPI spec (`openapi.json`) generated from the decree server's grpc-gateway. When the API changes:

1. Regenerate in decree: `make docs`
2. Copy to decree-ui: update `openapi.json`
3. Regenerate the TypeScript client: `npm run generate-api`

## Current Versions

| Repo | Latest | Next |
|------|--------|------|
| decree | v0.9.0-alpha.1 (GitHub Release + Go proxy) | v0.10.0-alpha.1 |
| decree-python | v0.2.0a1 (PyPI: opendecree) | v0.3.0a1 |
| decree-typescript | v0.2.0-alpha.1 (npm: @opendecree/sdk) | v0.3.0-alpha.1 |
| decree-ui | v0.1.0-alpha.1 (GHCR Docker image) | v0.2.0-alpha.1 |
| demos | (no releases) | — |
