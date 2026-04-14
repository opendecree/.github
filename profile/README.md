# OpenDecree

Schema-driven business configuration management. Define typed configuration schemas, manage per-tenant values, track every change.

## What is it?

OpenDecree is an open-source configuration management platform built for **business configuration** — feature flags, pricing rules, tenant settings, operational parameters — not infrastructure config. It provides:

- **Typed schemas** — define fields with types (integer, string, bool, time, duration, url, json), constraints, and metadata
- **Multi-tenancy** — isolated configuration per tenant with shared schemas
- **Full audit trail** — every change tracked with actor, timestamp, and diff
- **gRPC API** — fast, type-safe, with OpenAPI/REST gateway
- **SDKs** — Go, Python, TypeScript with real-time config watching

## Repositories

| Repo | Description |
|------|-------------|
| [decree](https://github.com/opendecree/decree) | Core service, Go SDKs, CLI |
| [decree-python](https://github.com/opendecree/decree-python) | Python SDK — [opendecree on PyPI](https://pypi.org/project/opendecree/) |
| [decree-typescript](https://github.com/opendecree/decree-typescript) | TypeScript SDK — [@opendecree/sdk on npm](https://www.npmjs.com/package/@opendecree/sdk) |
| [decree-ui](https://github.com/opendecree/decree-ui) | Admin GUI — React + Tailwind |

## Status

Alpha — all projects are under active development and subject to change.

## License

Apache 2.0
