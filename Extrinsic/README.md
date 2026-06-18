# Master Workflow Automation Script

A virtual screening platform by LK Innovations for small molecule discovery mentioned in the manuscript 

## **AI-enabled discovery of small molecules targeting complementary pathways for hair follicle rejuvenation**

This master script orchestrates all screening workflows: 
 - HMSA, GraphDTA, ColdDTA
 - Boltz2, Vina, DiffDock, and MD+PBSA.

The packages needed by the platform orchestration are described in the requirements.txt. 

The packages needed for involved methods are described in details in corresponding git repositories.

## Overview

The `run_all_workflows.sh` script provides a unified interface to:
- Run all workflows with a single command
- Run individual workflows selectively
- Configure task root and protein list from command line
- Check overall status of all workflows
- Pass configuration to all sub-scripts automatically

## Quick Start

The workflow is designed to execute in parallel on Slurm to maximize the efficiency. Please find all change the slurm specs before using the workflows.

```bash
# Run with an example
./orchestration/orchestration/run_all_workflows.sh --task-root ./examples/PHD2 --proteins "PHD2"

# Run with custom task root and proteins
./orchestration/run_all_workflows.sh --task-root /path/to/data --proteins "PROTEIN1 PROTEIN2 PROTEIN3"

# Check status of all workflows
./orchestration/run_all_workflows.sh status

# Show help
./orchestration/run_all_workflows.sh help
```

## Configuration

Edit the script to set default values (lines 12-16):

```bash
# Task root directory
TASK_ROOT="YOUR_TASK_FOLDER"

# Protein list (space-separated)
PROTEINS="PROTEIN1 PROTEIN2 PROTEIN3"
```

Or override from command line:

```bash
./orchestration/run_all_workflows.sh --task-root "YOUR_TASK_FOLDER" --proteins "PROTEIN1 PROTEIN2"
```

## Commands

### Run All Workflows (default)

```bash
./orchestration/run_all_workflows.sh
# or explicitly
./orchestration/run_all_workflows.sh all
```

Runs all enabled workflows in sequence:
1. Boltz2 (40 batches)
2. Vina (per-part jobs)
3. DiffDock (per-part jobs)
4. MD+PBSA (per-compound jobs)

### Run Individual Workflows

```bash
# Run only Boltz2
./orchestration/run_all_workflows.sh boltz2

# Run only Vina
./orchestration/run_all_workflows.sh vina

# Run only DiffDock
./orchestration/run_all_workflows.sh diffdock

# Run only MD+PBSA
./orchestration/run_all_workflows.sh md_pbsa
```

### Skip Workflows

```bash
# Run all except DiffDock and MD+PBSA
./orchestration/run_all_workflows.sh --skip-diffdock --skip-md-pbsa

# Run only Boltz2 and Vina
./orchestration/run_all_workflows.sh --skip-diffdock --skip-md-pbsa
```

### Check Status

```bash
./orchestration/run_all_workflows.sh status
```

Shows:
- Completion status for each workflow
- Number of completed jobs vs total
- SLURM queue status (running and pending jobs)

Example output:
```
Workflow Status
==========================================

Protein: PROTEIN1
----------------------------------------
  Boltz2: 35/40 batches completed
  Vina: 8/10 parts completed
  DiffDock: 6/10 parts completed
  MD: 45/102 completed
  PBSA: 40/102 completed

SLURM Queue Status:
----------------------------------------
Running jobs:
  boltz2_PROTEIN1_b36 RUNNING

Pending jobs:
  boltz2_PROTEIN1_b37 PENDING
```

## Options

### --task-root DIR
Set the task root directory. All protein subdirectories are expected under this path.

```bash
./orchestration/run_all_workflows.sh --task-root /mnt/data/screening
```

### --proteins LIST
Set the protein list (space-separated). The master script will process all listed proteins.

```bash
./orchestration/run_all_workflows.sh --proteins "PROTEIN1"
```

### --skip-[workflow]
Skip specific workflows:
- `--skip-boltz2` - Skip Boltz2
- `--skip-vina` - Skip Vina
- `--skip-diffdock` - Skip DiffDock
- `--skip-md-pbsa` - Skip MD+PBSA

```bash
# Run only Vina and DiffDock
./orchestration/run_all_workflows.sh --skip-boltz2 --skip-md-pbsa
```

## How It Works


### Workflow Execution

Each workflow is executed by calling its respective script:
1. `run_boltz2_batch.sh` - Submits 40 batch jobs
2. `run_vina_batch.sh` - Submits jobs for each CSV part
3. `run_diffdock_batch.sh` - Submits jobs for each CSV part
4. `run_md_pbsa_batch.sh` - Submits combined MD+PBSA jobs for each compound

## Directory Structure

Expected directory structure:

```
${TASK_ROOT}/
├── Input/
│   ├── sequences.csv
│   └── protein_file/
│       └── ${PROTEIN}/
│           ├── ${PROTEIN}.pdb
│           ├── ${PROTEIN}.gro
│           └── system_EM.top
└── ${PROTEIN}/
    ├── initial_screening/
    │   └── selected.csv
    └── fine_screening/
        ├── Boltz2/
        ├── Vina/
        └── PBSA/
            ├── DiffDock/
            └── PBSA/
                ├── MD/
                └── PBSA/
```

## Example Workflows

### Scenario 1: First time run with all workflows

```bash
# Run everything
./orchestration/run_all_workflows.sh --task-root /data/screening --proteins "PROTEIN1 PROTEIN2"

# Monitor progress
watch -n 60 ./orchestration/run_all_workflows.sh status
```

### Scenario 2: Run only docking methods (skip structure prediction)

```bash
# Skip Boltz2, run only Vina and DiffDock
./orchestration/run_all_workflows.sh --skip-boltz2 --skip-md-pbsa

# Or run them individually
./orchestration/run_all_workflows.sh vina
./orchestration/run_all_workflows.sh diffdock
```

### Scenario 3: Multiple proteins with selective workflows

```bash
# Run Boltz2 and Vina for three proteins
./orchestration/run_all_workflows.sh \
    --task-root /mnt/data \
    --proteins "PROTEIN1 PROTEIN2" \
    --skip-diffdock \
    --skip-md-pbsa
```

