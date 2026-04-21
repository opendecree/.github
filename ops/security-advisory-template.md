# Security Advisory Template

Draft template for maintainers when creating a security advisory via `/<repo>/security/advisories/new`. Copy-paste and fill in.

## Title

`<one-line summary — e.g., "Path traversal in SchemaService.UploadFile">`

## Severity

Select one — use the [CVSS calculator](https://www.first.org/cvss/calculator/3.1) if unsure:

| Severity | CVSS range | Examples |
|---|---|---|
| Critical | 9.0–10.0 | RCE, auth bypass to admin, full data exfiltration |
| High | 7.0–8.9 | Priv-esc, partial data exfiltration, persistent XSS |
| Medium | 4.0–6.9 | Info disclosure, DoS, reflected XSS |
| Low | 0.1–3.9 | Minor info leak, non-exploitable edge case |

## Affected products

- Package / artifact name
- Affected version range (e.g., `>=0.1.0, <0.8.1`)
- Patched version (if known)

## Description

- **What** — one paragraph describing the vulnerability
- **Impact** — what an attacker can achieve
- **Prerequisites** — auth level, network position, configuration required

## Reproduction

Minimal steps or PoC. Redact any production data.

```
# step 1
# step 2
```

## Mitigation

Temporary workaround while the fix lands (e.g., disable a feature, add a WAF rule).

## Fix

- Commit SHA or PR link
- Summary of the change
- Regression test added (yes/no + link)

## Credits

- Reporter name + affiliation (ask permission before naming)
- Internal responders

## Timeline

| Date | Event |
|---|---|
| YYYY-MM-DD | Report received |
| YYYY-MM-DD | Fix landed on main |
| YYYY-MM-DD | Patched release published |
| YYYY-MM-DD | Advisory made public |

## Checklist before publishing

- [ ] CVE requested (via GitHub's advisory form)
- [ ] Affected versions verified on real tags
- [ ] Patched release published to package registry
- [ ] Release notes reference the CVE
- [ ] Dependents notified where practical
