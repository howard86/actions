# Minimal fixture

A throwaway Bun + Turbo workspace used only by `.github/workflows/test.yml` to
exercise the `setup` and `quality` composites and the reusable `ci.yml` end to
end. The package scripts are trivial echoes — the point is to verify the
actions' wiring, not a real toolchain. `typecheck`/`test`/`build` route through
`turbo run` so the self-test actually populates `.turbo` and validates the cache
path the actions ship; `check` is a root script, mirroring how `ultracite check`
runs repo-wide in the real consumers.

`knip` is a real dependency here, not an echo — `ci.yml` runs it
unconditionally, so the fixture has to satisfy it. It needs no `knip.json`:
with nothing to analyse, knip's defaults pass clean. Real consumers will need
one.
