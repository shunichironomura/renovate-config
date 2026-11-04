# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for `shunichironomura` repositories.
`graphcal-lang` has its own copy at [graphcal-lang/renovate-config](https://github.com/graphcal-lang/renovate-config).

## Usage

```json5
{
  $schema: "https://docs.renovatebot.com/renovate-schema.json",
  extends: ["github>shunichironomura/renovate-config"],
}
```

Repositories that pin GitHub Actions through `.github/workflows/actions.lock` also extend
`github>shunichironomura/renovate-config:actions-lock`.

## Policy (`default.json`)

| Update | Behavior |
|---|---|
| Security fixes | Immediate PR (Renovate's `vulnerabilityAlerts` ignores `schedule` and `minimumReleaseAge`). Requires Dependabot alerts on the repository |
| Major | Individual PR, weekly, no automerge |
| 0.x minor | Grouped PR, weekly, no automerge |
| Other minor / patch / digest / pin | One grouped PR, weekly, automerged when CI passes |
| Lock file maintenance | Weekly, automerged when CI passes |

- Schedule: early Monday mornings (Asia/Tokyo)
- Cooldown: `minimumReleaseAge: "7 days"`; automerge waits for the `renovate/stability-days` check
- pre-commit hooks are updated by Renovate (`:enablePreCommit`), not pre-commit.ci

Automerge is safe only where the default branch requires status checks. Without required checks,
GitHub's native automerge may merge before CI completes.

## Validation

```sh
npx --yes --package renovate@latest -- renovate-config-validator --strict default.json actions-lock.json
```
