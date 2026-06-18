# Scripts Layout

This directory is organized by purpose. Canonical implementations live in these subfolders:

- `orchestration/`: high-level pipeline orchestrators.
- `prepare_input/`: input generation and data splitting utilities.
- `initial_screening/`: GraphDTA/HMSA/ColdDTA  submitters.
- `structure_prediction/`: Boltz2/ prefold and batch jobs.
- `docking/`: Vina and DiffDock job runners and docking utilities.
- `scoring/`: summary/result score collectors.
- `md_pbsa/`: MD+PBSA batch runner and PBSA helper scripts (`md_pbsa/pbsa/`).
- `monitoring/`: progress/status monitoring scripts.

## Backward Compatibility

Top-level executable script names in `scripts/` are preserved as thin wrappers that forward to canonical files in subfolders. Existing commands such as `./scripts/run_full_pipeline.sh` remain valid.
