# CLAUDE.md

## Project Overview

Base template for data science projects. Provides a standardized structure with reproducible pipelines, data versioning (DVC), experiment tracking (MLflow), and code quality tooling.

## Directory Structure

- `constants/paths.py` — Centralized path definitions (PROJECT_ROOT, BRONZE_DIR, SILVER_DIR, GOLD_DIR, etc.)
- `config/` — Configuration files (e.g. `loguru.yaml`)
- `data/` — Bronze (raw) → Silver (cleaned) → Gold (features) data layer pattern
- `src/pipelines/data/` — Data transformation pipeline modules (bronze_to_silver, silver_to_gold)
- `src/pipelines/models/` — Model training pipeline modules
- `src/pipelines/utils/` — Shared pipeline utilities
- `src/app/` — Streamlit dashboard
- `src/utils/logger.py` — Loguru logger: `from src.utils.logger import logger`
- `dvc.yaml` — DVC pipeline stage definitions
- `params.yaml` — Tunable parameters tracked by DVC

## Key Conventions

- **Python 3.13**, managed with **uv**
- All commands prefixed with `uv run` (e.g. `uv run dvc repro`, `uv run streamlit run ...`)
- Pipeline modules use `__main__` entry points for DVC (run via `uv run python -m src.pipelines.data.bronze_to_silver`)
- Paths are centralized in `constants/paths.py` — never hardcode paths
- Logger: `from src.utils.logger import logger`

## Linting & Formatting

- **Ruff** with line-length 88, Google docstring convention
- Selected rules: E, W, F, I, D, UP, B (ignoring D100, D104)
- Pre-commit hooks: ruff (lint + format), nbstripout, uv-lock, trailing-whitespace, end-of-file-fixer, check-toml, check-yaml, check-json, check-added-large-files

## DVC Pipeline

- Stages defined in `dvc.yaml`, parameters in `params.yaml`
- Run full pipeline: `uv run dvc repro`
- Run single stage: `uv run dvc repro <stage_name>`
- Run experiments: `uv run dvc exp run --set-param key=value`

## MLflow

- Backend: Databricks (configured via env vars in `.env`)
- Env vars: `DATABRICKS_HOST`, `DATABRICKS_TOKEN`, `MLFLOW_EXPERIMENT_ID`, `MLFLOW_TRACKING_URI`

## CI

- GitHub Actions (`.github/workflows/ci.yaml`) runs pre-commit on push/PR
