# actions

Shared GitHub Actions for Bun + Turbo monorepos. This repo is the single source
of truth for third-party action versions — consumers reference the building
blocks here instead of pinning `actions/checkout`, `oven-sh/setup-bun`,
`crate-ci/typos`, `gitleaks/gitleaks-action`, etc. in every repo.

It exposes two layers:

- **Composite actions** — `setup` and `quality` — step-level building blocks you
  drop into your own jobs.
- **A reusable workflow** — `.github/workflows/ci.yml` — the batteries-included
  full-CI job that composes the same steps. Adopt it wholesale for a simple
  repo, or use the composites à la carte for a complex one.

## How consumers pin

Pin to a **commit SHA** with a trailing version comment, and let Dependabot /
Renovate (already configured in the consumer repos for the `github-actions`
ecosystem) bump it:

```yaml
uses: howard86/actions/setup@<sha> # v1.0.0
```

Releases here are cut by release-please as `vX.Y.Z` tags; your updater resolves
the SHA from those tags.

## Reusable workflow: `ci.yml`

```yaml
# .github/workflows/ci.yml in a consumer repo
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: howard86/actions/.github/workflows/ci.yml@<sha> # v1.0.0
    permissions:
      contents: read
    with:
      node-version: "24"        # omit to skip Node setup
      run-test: true
      run-build: ${{ github.event_name == 'push' && github.ref == 'refs/heads/main' }}
    secrets:
      gitleaks-license: ${{ secrets.GITLEAKS_LICENSE }}   # optional
```

Runs, in order: checkout (fetch-depth 0) → Bun (+ optional Node) → Turbo & Bun
caches → `bun install --frozen-lockfile` → `bun run check` → typos → gitleaks →
actionlint → `bun run typecheck` → `bun run test` → `bun run build`.

| Input | Type | Default | Notes |
|-------|------|---------|-------|
| `node-version` | string | `""` | Empty skips Node setup. |
| `run-test` | boolean | `true` | Set `false` if the repo has no tests. |
| `run-build` | boolean | `true` | Pass an expression to build on `main` only. |
| `working-directory` | string | `"."` | Install/run scripts in a subdirectory. |

| Secret | Required | Notes |
|--------|----------|-------|
| `gitleaks-license` | no | Only for gitleaks org/enterprise features. |
| `GITHUB_TOKEN` | — | Provided automatically to the called workflow. |

Requires these root scripts in the consumer: `check`, `typecheck`, `test`,
`build`. (`bun run check` is `ultracite check` in all current consumers.)

## Composite: `setup`

Sets up Bun (and optionally Node), restores the Turbo and Bun caches, and runs a
frozen install. Assumes the repo is already checked out.

```yaml
- uses: actions/checkout@<sha>
- uses: howard86/actions/setup@<sha> # v1.0.0
  with:
    node-version: "24"   # optional
```

| Input | Default | Notes |
|-------|---------|-------|
| `node-version` | `""` | Empty skips Node setup. |
| `working-directory` | `"."` | Directory holding `package.json` / `bun.lock`. |

## Composite: `quality`

Spell-check (typos) + secret scan (gitleaks) + workflow lint (actionlint). Uses a
repo-local config when present (`_typos.toml`/`typos.toml`/`.typos.toml`,
`.gitleaks.toml`/`gitleaks.toml`), otherwise the bundled default in
`quality/configs/`. **Check out with `fetch-depth: 0`** so gitleaks can scan
history.

```yaml
- uses: actions/checkout@<sha>
  with:
    fetch-depth: 0
- uses: howard86/actions/quality@<sha> # v1.0.0
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

| Input | Required | Default | Notes |
|-------|----------|---------|-------|
| `github-token` | yes | — | Pass `secrets.GITHUB_TOKEN` (gitleaks needs it). |
| `gitleaks-license` | no | `""` | gitleaks org/enterprise license. |
| `typos-config` | no | `""` | Override config path. |
| `gitleaks-config` | no | `""` | Override config path. |

## Repo layout

```
setup/action.yml              composite: Bun/Node setup + caches + install
quality/action.yml            composite: typos + gitleaks + actionlint
quality/configs/              bundled fallback typos.toml / gitleaks.toml
.github/workflows/ci.yml      reusable full-CI workflow
.github/workflows/test.yml    self-test: runs the above against fixtures/minimal
.github/workflows/release-please.yml   cuts vX.Y.Z tags + releases
fixtures/minimal/             tiny Bun + Turbo workspace for the self-test
```

## Releasing

Conventional-commit to `main`; release-please opens a release PR. Merging it
tags `vX.Y.Z` and publishes a GitHub release. Consumers' updaters then pick up
the new SHA.
