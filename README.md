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

The preset assumes GitHub Actions are pinned through `.github/workflows/actions.lock`
(`gh actions-lock`), so it turns off inline digest pinning. A repository that has not adopted the
lockfile yet adds `"helpers:pinGitHubActionDigestsToSemver"` after the preset in its own `extends`,
and removes it when it adopts the lockfile.

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

Renovate merges the PR itself (`platformAutomerge: false`) after every status check is green,
including `renovate/stability-days`. GitHub's native auto-merge only waits for the checks that
branch protection requires, so it would merge updates younger than `minimumReleaseAge` as soon as
CI passes. Renovate merges on its own runs (usually hourly), so merging lags CI by up to an hour.

## Validation

```sh
npx --yes --package renovate@latest -- renovate-config-validator --strict default.json
```
