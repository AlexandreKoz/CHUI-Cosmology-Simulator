# Developer workflow contract

This document specifies the expected review evidence for changes to the CHUÍ scientific software stack.

## Implementation summary and evidence

Each substantive change should describe:

1. Purpose, design and exact files affected.
2. Numerical assumptions, physical conventions and important invariants.
3. Ownership and persistence impacts: configuration, canonical runtime state, derived arrays, restart data, MPI ghost state and output schema as applicable.
4. Added or updated tests, with exact commands and actual outcomes.
5. Performance impacts and benchmark or profiling evidence for hot paths.
6. Known limitations, blocked checks and outstanding scientific validation.

## Engineering rules

- No placeholder logic in production numerical paths.
- No unrelated rewrites in narrowly scoped patches.
- No undocumented changes to configuration, numerical conventions, schema or provenance.
- Avoid implicit unit or coordinate-frame conversions.
- Do not package local build trees, caches, generated simulation data or binaries in source releases.
- Prefer clear, reviewable changes with a reproducible validation procedure.

## Documentation updates

Changes to configuration, output and restart schemas, validation expectations, build workflows, or benchmarks should update their corresponding documentation in the same change. Architecture-level design decisions belong in `docs/architecture/decision_log.md`.
