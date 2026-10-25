# Contributing to CHUÍ

CHUÍ is scientific computing software. Changes to numerical methods, data ownership, and serialization must prioritize correctness, reproducibility, and explicit validation.

## Development workflow

1. Describe the problem, expected behavior, and any numerical or physical assumptions.
2. Keep each change focused, with clear ownership of state and memory.
3. Add or update relevant unit, integration, or validation tests.
4. Run the smallest relevant CMake preset and record the exact commands and results. Note missing optional dependencies or tests that were not executed.
5. Update documentation when configuration, output formats, numerical conventions, or public interfaces change.
6. Supply benchmarks or profiling evidence for modifications to performance-sensitive paths.

## Conventions

- C++20 for the simulation core; preserve the existing `cosmosim` compatibility namespaces and schema identifiers unless a versioned migration is planned.
- Use `lower_snake_case` for files and local variables, `PascalCase` for types, and `camelCase` for methods, following the existing module conventions.
- Make coordinate frame and unit conventions explicit, especially for cosmological and physical quantities.
- Keep canonical state separate from derived caches, temporary scratch buffers, and distributed ghost state.
- Avoid unrelated refactors in fixes to numerical algorithms or serialization.

## Before submitting changes

```bash
bash scripts/ci/check_repo_hygiene.sh
cmake --preset cpu-only-debug
cmake --build --preset build-cpu-debug
ctest --preset test-cpu-debug --output-on-failure
```

Additional HDF5, FFTW, MPI, and other dependency-enabled checks are described in [build instructions](docs/build_instructions.md). See the [developer workflow](docs/architecture/developer_workflow_contract.md), [configuration](docs/configuration.md), and [validation plan](docs/validation_plan.md) for interfaces and acceptance criteria.

Do not report an unexecuted test as passing. Document observed results and remaining validation limitations.
