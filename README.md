# Base Data Science Project Setup

<!-- Build & CI Status -->
![CI](https://github.com/auggy-ntn/base-data-science-project-setup/actions/workflows/ci.yaml/badge.svg?event=push)

<!-- Code Quality & Tools -->
[![Code style: ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

<!-- Environment & Package Management -->
![Python Version](https://img.shields.io/badge/python-3.13+-blue.svg)
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)

A reusable template for data science projects with reproducible pipelines, experiment tracking, and data versioning baked in.

## Project Structure

```
.
├── config/                    # Configuration files (e.g. loguru.yaml)
├── constants/
│   └── paths.py               # Centralized path definitions
├── data/
│   ├── bronze/                # Raw, immutable data
│   ├── silver/                # Cleaned, validated data
│   └── gold/                  # Feature-engineered, model-ready data
├── docs/
│   ├── DVC_WORKFLOW.md        # Data versioning guide
│   ├── PROJECT_OWNER_CHECKLIST.md  # Owner setup checklist
│   └── SETUP.md               # Team onboarding guide
├── notebooks/                 # Exploratory notebooks
├── src/
│   ├── app/                   # Streamlit dashboard
│   ├── pipelines/
│   │   ├── data/              # bronze_to_silver, silver_to_gold
│   │   ├── models/            # Model training scripts
│   │   └── utils/             # Shared pipeline utilities
│   └── utils/
│       └── logger.py          # Loguru logger setup
├── dvc.yaml                   # DVC pipeline stages
├── params.yaml                # Tunable parameters (tracked by DVC)
├── pyproject.toml             # Project metadata & dependencies
└── .pre-commit-config.yaml    # Pre-commit hook configuration
```

## Tools & Stack

| Tool | Purpose |
|------|---------|
| [uv](https://docs.astral.sh/uv/) | Package & environment management |
| [DVC](https://dvc.org/) | Data versioning & pipeline orchestration |
| [MLflow](https://mlflow.org/) | Experiment tracking (Databricks backend) |
| [Ruff](https://docs.astral.sh/ruff/) | Linting & formatting |
| [pre-commit](https://pre-commit.com/) | Git hooks (ruff, nbstripout, uv-lock, file checks) |
| [Loguru](https://loguru.readthedocs.io/) | Logging |
| [Streamlit](https://streamlit.io/) | Dashboard / app |

## Quick Start

```bash
# 1. Clone
git clone https://github.com/auggy-ntn/base-data-science-project-setup.git
cd base-data-science-project-setup

# 2. Install dependencies
uv sync

# 3. Set up pre-commit hooks
uv run pre-commit install

# 4. Configure environment variables
cp .env.example .env
# Edit .env with your DVC and MLflow credentials

# 5. Configure DVC remote
source .env
dvc remote modify --local ovh-storage access_key_id $DATA_ACCESS_KEY_ID
dvc remote modify --local ovh-storage secret_access_key $DATA_SECRET_ACCESS_KEY

# 6. Pull data
dvc pull

# 7. Run the pipeline
uv run dvc repro
```

See [docs/SETUP.md](docs/SETUP.md) for detailed setup instructions.

## Data Layer Pattern

This template uses a **bronze / silver / gold** data layer pattern:

- **Bronze** — Raw, immutable source data. Never modify directly.
- **Silver** — Cleaned and validated data (typically Parquet).
- **Gold** — Feature-engineered, model-ready datasets.

## Adapting This Template

To use this template for a new project:

1. **Rename the project** in `pyproject.toml` (`name`, `description`)
2. **Update the DVC remote** — create a new storage bucket and update `.dvc/config`
3. **Update MLflow** — create a new Databricks experiment and update `MLFLOW_EXPERIMENT_ID` in `.env.example`
4. **Update the CI badge** URL in this README to point to your repo
5. **Add your data** to `data/bronze/` and track with `dvc add`
6. **Implement your pipelines** in `src/pipelines/data/` and `src/pipelines/models/`
7. **Define pipeline stages** in `dvc.yaml` and parameters in `params.yaml`

## Documentation

- [Team Setup Guide](docs/SETUP.md) — Full onboarding instructions
- [DVC Workflow](docs/DVC_WORKFLOW.md) — Data versioning and pipeline guide
- [Project Owner Checklist](docs/PROJECT_OWNER_CHECKLIST.md) — Infrastructure setup for project owners
