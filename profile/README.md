<p align="center">
  <img src="https://raw.githubusercontent.com/opendecree/decree/main/assets/logo.png" alt="OpenDecree" width="150">
</p>

<h1 align="center">OpenDecree</h1>

<p align="center"><strong>Stop hard-coding fees, limits, and business rules — and redeploying every time one changes.</strong></p>

<p align="center"><em>Typed business configuration: the layer between feature flags and infrastructure config.</em></p>

> **Alpha** — all OpenDecree projects are under active development and subject to change. Not recommended for production use yet.

<p align="center">
  <img src="https://raw.githubusercontent.com/opendecree/decree/main/assets/demo.gif" alt="OpenDecree quickstart demo: schema → config → watch" width="800">
</p>

<p align="center">
  <a href="https://github.com/opendecree/demos"><strong>&raquo; Try it in 5 minutes — opendecree/demos</strong></a>
</p>

---

## Schema → APIs → SDKs

Define a field once:

```yaml
# payments.decree.schema.yaml
fields:
  payments.fee:
    type: number
    constraints: { min: 0.0, max: 1.0 }
```

Get a typed, validated read everywhere:

```go
// Go
fee, _ := client.GetFloat(ctx, tenantID, "payments.fee") // float64
```

```python
# Python
fee = client.get(tenant_id, "payments.fee", float)
```

```typescript
// TypeScript
const fee = await client.get(tenantId, "payments.fee", Number);
```

---

## Why OpenDecree?

OpenDecree sits between **feature flags** (release toggles) and **infrastructure config** (low-level key-value) — purpose-built for typed business config: fees, limits, approval rules, settlement windows.

| Capability | Feature Flags | Infra Config | **OpenDecree** |
|---|---|---|---|
| Typed values | bool / variant only | strings only | ✓ int, number, string, bool, time, duration, url, json |
| Multi-tenant | limited (segments) | ✗ | ✓ first-class, with field-level locking |
| Audit trail + rollback | limited | ✗ | ✓ full history, rollback to any state |

---

## Repositories

| Repo | Description |
|------|-------------|
| [decree](https://github.com/opendecree/decree) | Core service, Go SDKs, CLI |
| [decree-python](https://github.com/opendecree/decree-python) | Python SDK — [opendecree on PyPI](https://pypi.org/project/opendecree/) |
| [decree-typescript](https://github.com/opendecree/decree-typescript) | TypeScript SDK — [@opendecree/sdk on npm](https://www.npmjs.com/package/@opendecree/sdk) |
| [decree-ui](https://github.com/opendecree/decree-ui) | Admin GUI — React + Tailwind |
| [demos](https://github.com/opendecree/demos) | Hands-on examples — try OpenDecree in 5 minutes |

## Status

Alpha — all projects are under active development and subject to change. See the **[Roadmap](https://github.com/opendecree/decree/blob/main/ROADMAP.md)** for what is stable today and the path to a 1.0 stable release.

## Community

Questions, ideas, show-and-tell — [OpenDecree Discussions](https://github.com/orgs/opendecree/discussions) is our single community hub across all repos.

See [GOVERNANCE.md](https://github.com/opendecree/.github/blob/main/GOVERNANCE.md) for how the project is governed.

## License

Apache 2.0
