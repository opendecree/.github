# Security Policy

This is the default security policy for all [OpenDecree](https://github.com/opendecree) repositories. Individual repositories may publish their own `SECURITY.md` with additional detail; where one exists, it takes precedence.

## Reporting a Vulnerability

**Do not open a public GitHub issue for a security vulnerability.**

Report it privately through GitHub Security Advisories on the affected repository:

| Repository | Report |
|---|---|
| Core service, CLI, Go SDKs | [decree](https://github.com/opendecree/decree/security/advisories/new) |
| Python SDK | [decree-python](https://github.com/opendecree/decree-python/security/advisories/new) |
| TypeScript SDK | [decree-typescript](https://github.com/opendecree/decree-typescript/security/advisories/new) |
| Admin GUI | [decree-ui](https://github.com/opendecree/decree-ui/security/advisories/new) |
| Examples | [demos](https://github.com/opendecree/demos/security/advisories/new) |

If you aren't sure which repository is affected, report against [decree](https://github.com/opendecree/decree/security/advisories/new) and we'll route it.

Please include:

1. A description of the vulnerability
2. Steps to reproduce
3. The potential impact
4. Any suggested fix (optional)

You should receive a response within 48 hours. We will work with you to understand and address the issue before any public disclosure.

## Alpha status

All OpenDecree projects are **alpha** and are not recommended for production use. During alpha, only the latest release of a given repository receives security fixes — there are no backports to earlier alpha releases. Current published versions are listed in the [Current Versions table](https://github.com/opendecree/.github/blob/main/RELEASING.md#current-versions).

This does not change how we handle reports: alpha status is a reason to report early, not a reason to skip reporting.

## Supply chain

Release binaries and Docker images are signed with [Sigstore](https://sigstore.dev) via GitHub Actions artifact attestations ([SLSA](https://slsa.dev) provenance). Verification instructions are in the affected repository's own `SECURITY.md` — see [decree](https://github.com/opendecree/decree/blob/main/SECURITY.md) for the canonical example.

## Scope

This policy covers code in OpenDecree repositories. It does not cover third-party dependencies (report those upstream), nor deployments of OpenDecree operated by someone else.
