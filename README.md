# CHUÍ

**CHUÍ** is a modular cosmological simulation framework written in C++20. It is designed for dark-matter and baryonic structure-formation experiments on workstations and small HPC clusters, with explicit attention to memory use, reproducibility, and parallel execution.

The **dark-matter-only (DMO) cosmological path** is the current operational milestone. Hydrodynamics, adaptive mesh refinement (AMR), and several baryonic/subgrid modules are present in the source tree but must be independently validated before they are used for quantitative scientific results.

## Main capabilities

| Subsystem | Implementation and scope |
| --- | --- |
| Gravity | TreePM coupling of short-range tree forces and long-range particle-mesh gravity, including periodic cosmological boundaries |
| Cosmological evolution | Configuration-driven scale-factor integration; conservative global timestep mode is the established first-light path |
| Initial conditions | GADGET/AREPO-style HDF5 particle initial conditions, including tested monofonIC-style DMO inputs |
| Parallel execution | OpenMP and MPI pathways; FFTW/FFTW-MPI and Parallel HDF5 are available through dependency-enabled builds |
| Outputs | HDF5 snapshots, normalized configuration, provenance data, and transactional completion markers |
| Memory management | Structure-of-arrays state layout, explicit resource ownership, and memory governance |
| Additional physics | AMR, Godunov hydrodynamics, cooling, star formation, feedback, metals, and black-hole modules at differing stages of implementation and validation |

The project name is **CHUÍ**; `cosmosim` and `CosmoSim` remain in internal namespaces, binary names, and schema identifiers for compatibility.

## Scientific status

A 32³ periodic cosmological DMO first-light run completed through the normal simulation workflow, including external HDF5 initial-condition ingestion, TreePM force evaluation, time integration, and science snapshot output. This is evidence that the end-to-end path operates; **it is not, on its own, a convergence study or certification of the physical accuracy of production-scale simulations.**

Before using results for quantitative scientific conclusions, the relevant configurations still require checks of force accuracy, linear growth, power spectra, resolution convergence, reproducibility, rank-count equivalence, and resource scaling. Distributed modes, hierarchical timestepping, and baryonic physics have separate validation boundaries. The code is provided as research software under active development, not as a turnkey validated production package.

See [validation](docs/validation_plan.md), [gravity](docs/tree_pm_coupling.md), and [known issues](docs/releases/known_issues.md).

## Build

CHUÍ uses CMake and a C++20 compiler. Optional build dependencies include HDF5, FFTW3, MPI, and OpenMP. See the [build guide](docs/build_instructions.md) for dependency-specific requirements and platforms.

A dependency-light developer build:

```bash
cmake --preset cpu-only-debug
cmake --build --preset build-cpu-debug
ctest --preset test-cpu-debug --output-on-failure
```

A TreePM/HDF5/FFTW development build:

```bash
cmake --preset pm-hdf5-fftw-debug
cmake --build --preset build-pm-hdf5-fftw-debug
```

The source tree includes `./chui`, a convenience launcher, and `cosmosim_harness`, the native runtime executable.

## Running a DMO example

The sample deck is [`configs/chui_firstlight_32_smoke.param.txt`](configs/chui_firstlight_32_smoke.param.txt). It expects an externally supplied initial-condition file at `ics/chui_ic_32_10mpch_z24.hdf5` relative to the repository root; **that file is not distributed here.** Provide a compatible file or update the parameter deck to point to your own initial conditions.

```bash
./chui run configs/chui_firstlight_32_smoke.param.txt \
  --preset pm-hdf5-fftw-debug
```

If the launcher is not executable after archive extraction, invoke it using Python:

```bash
python3 ./chui run configs/chui_firstlight_32_smoke.param.txt \
  --preset pm-hdf5-fftw-debug
```

Run configuration, cosmological parameters, units, snapshot schedule, and parallel settings are defined in typed `.param.txt` files. For MPI builds, use a preset with MPI, FFTW-MPI, and (for collective single-file snapshots) Parallel HDF5 support.

## Reproducibility and outputs

CHUÍ records normalized configuration and provenance alongside HDF5 science products. Completed snapshot sets carry `.complete` markers so that partially written outputs are not confused with committed snapshots. Restart/checkpoint files have separate formats and validation rules. Scientific accuracy and reproducibility depend on the selected numerical options, initial conditions, compiler/build configuration, and validation coverage.

See [configuration](docs/configuration.md), [output schema](docs/output_schema.md), [restart/checkpointing](docs/restart_checkpointing.md), and [units and cosmology](docs/units_cosmology_provenance.md).

## Documentation

- [Architecture and state ownership](docs/architecture/overview.md)
- [Tree gravity solver](docs/tree_gravity_solver.md)
- [Particle-mesh gravity](docs/pm_gravity_solver.md)
- [TreePM coupling](docs/tree_pm_coupling.md)
- [Hydrodynamics and AMR](docs/hydrodynamics.md)
- [Build instructions](docs/build_instructions.md)
- [Validation and reproducibility](docs/validation_plan.md)
- [Profiling and performance](docs/profiling.md)
- [Contributing](CONTRIBUTING.md)

The presence of a model or module in the repository does not imply validation for all parameter regimes.

## Licensing

**A redistribution license has not yet been selected for this public-preview snapshot.** The maintainer must add an appropriate `LICENSE` file after confirming the provenance and licensing of any third-party material. Until then, do not assume that this archive grants permission for third-party redistribution or modification.
