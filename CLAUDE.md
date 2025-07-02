# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SigProfilerSimulator is a Python package for realistic simulation of mutational signatures in cancer genomes. It supports single point mutations (SBS), double point mutations (DBS), and insertion/deletions (ID) across multiple reference genomes (GRCh37, GRCh38, mm9, mm10, etc.).

## Key Architecture

- **Main simulator**: `SigProfilerSimulator/SigProfilerSimulator.py` - Core simulation logic and entry point
- **Mutation engine**: `SigProfilerSimulator/mutational_simulator.py` - Low-level mutation simulation algorithms  
- **Dependencies**: Relies heavily on SigProfilerMatrixGenerator for reference genome handling and matrix operations

## Development Commands

### Installation and Setup
```bash
pip install .
```

### Testing
```bash
# Run basic test script
python3 test.py

# Run comprehensive test suite with pytest
pytest -s -rw tests

# Run specific test file
pytest tests/SPS_test.py
```

### Building
```bash
# Build package
python setup.py build

# Create distribution
python setup.py sdist bdist_wheel
```

## Test Architecture

The test suite (`tests/SPS_test.py`) focuses on reproducibility verification:
- Tests seed-based reproducibility across different configurations
- Validates that identical seeds produce identical outputs
- Tests both chromosome-based and genome-wide simulation modes
- Uses parameterized tests to verify behavior across multiple scenarios

Test data is organized in structured directories under `tests/` with expected outputs for comparison.

## Key Simulation Parameters

The main `SigProfilerSimulator()` function accepts:
- **Required**: `project`, `project_path`, `genome`, `contexts`
- **Critical for reproducibility**: `seed_file` - path to master seed file
- **Performance**: `chrom_based` - maintains chromosome-level catalogs
- **Customization**: `bed_file`, `region`, `mask` - for targeted simulations

## Reference Genome Setup

Before running simulations, reference genomes must be installed via SigProfilerMatrixGenerator:
```bash
SigProfilerMatrixGenerator install GRCh37 --local_genome ./src/
```

## Output Structure

Simulations create structured output directories:
- `input/` - Copies of input VCF/MAF files
- `output/` - Contains DBS, SBS, ID matrices and simulation results
- `logs/` - Error and progress logs
- `output/simulations/[project]_simulations_[genome]_[context]/` - Individual simulation files

## Reproducibility

The package supports deterministic simulations through seed files. The same seed file with identical parameters and CPU core count will produce identical results. Seeds are managed automatically but can be provided via `seed_file` parameter for reproducibility.