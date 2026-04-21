# Security Defaults

Baseline security configuration for the `opendecree` org and all public repos. Keep this doc in sync with reality — if you flip a toggle, update this file.

## Org-level (settings)

Managed at `github.com/organizations/opendecree/settings/security`.

| Setting | State | How to set |
|---|---|---|
| Require 2FA for all members | ✅ on | UI only — API field is silently ignored on Free plan |
| Secret scanning on new repos | ✅ on | `PATCH /orgs/opendecree` `secret_scanning_enabled_for_new_repositories=true` |
| Secret scanning push protection on new repos | ✅ on | `PATCH /orgs/opendecree` `secret_scanning_push_protection_enabled_for_new_repositories=true` |
| Dependabot alerts on new repos | ✅ on | `PATCH /orgs/opendecree` `dependabot_alerts_enabled_for_new_repositories=true` |
| Dependabot security updates on new repos | ✅ on | `PATCH /orgs/opendecree` `dependabot_security_updates_enabled_for_new_repositories=true` |

## Per-repo (public repos)

Applied to `decree`, `decree-python`, `decree-typescript`, `decree-ui`, `demos`.

| Setting | State | Notes |
|---|---|---|
| Private vulnerability reporting | ✅ on (all 5 repos) | `PUT /repos/{owner}/{repo}/private-vulnerability-reporting` |
| CodeQL default setup | ✅ on (decree, decree-python, decree-typescript, decree-ui) | demos skipped — no source code. `PATCH /repos/{owner}/{repo}/code-scanning/default-setup` with `{"state":"configured"}` |
| Branch protection on `main` | Enforced | `enforce_admins=true`, require PR, require status checks (incl. `Analyze (<lang>)` for CodeQL) |
| Auto-delete merged branches | Enforced | Repo Settings → General → Pull Requests |

### CodeQL required checks per repo

| Repo | Required `Analyze (*)` contexts |
|---|---|
| decree | `Analyze (go)`, `Analyze (actions)` |
| decree-python | `Analyze (python)`, `Analyze (actions)` |
| decree-typescript | `Analyze (javascript-typescript)`, `Analyze (actions)` |
| decree-ui | `Analyze (javascript-typescript)`, `Analyze (actions)` |

## Audit

Verify org-level state:

```bash
gh api /orgs/opendecree --jq '{
  two_factor_requirement_enabled,
  secret_scanning_enabled_for_new_repositories,
  secret_scanning_push_protection_enabled_for_new_repositories,
  dependabot_alerts_enabled_for_new_repositories,
  dependabot_security_updates_enabled_for_new_repositories
}'
```

Verify member 2FA compliance:

```bash
gh api '/orgs/opendecree/members?filter=2fa_disabled' --jq 'length'
# Expected: 0
```

Verify CodeQL state per repo:

```bash
for r in decree decree-python decree-typescript decree-ui; do
  echo -n "$r: "
  gh api /repos/opendecree/$r/code-scanning/default-setup --jq .state
done
# Expected: "configured" for all
```

## Related

- [opendecree/decree#160](https://github.com/opendecree/decree/issues/160) — org-level defaults
- [opendecree/decree#150](https://github.com/opendecree/decree/issues/150) — CodeQL default setup
- [opendecree/decree#151](https://github.com/opendecree/decree/issues/151) — private vulnerability reporting
- [opendecree/decree#149](https://github.com/opendecree/decree/issues/149) — merge hygiene toggles across repos
- [SECURITY.md](../SECURITY.md) — how to report vulnerabilities
