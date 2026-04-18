# Research Paper Implementations

## Scope
This repository is a working lab notebook for implementing and validating research papers in machine learning and computer science. Each paper is isolated in its own root-level directory with independent dependencies, metadata, and outputs.

## Repository Structure
- `/<paper_name>/` per paper implementation (self-contained project)
- No shared runtime assumptions between paper folders
- Heavy experiments are executed on Kaggle infrastructure rather than local hardware

## Reproducibility Protocol
Each paper directory should contain at minimum:
- `pyproject.toml` and `uv.lock` (dependency specification and lock state)
- execution notebook/script (`notebook.ipynb` or equivalent)
- `kernel-metadata.json` for Kaggle execution
- `outputs/` for downloaded run artifacts

## Dependency Management
Dependencies are managed with `uv`, scoped to each paper directory.

Typical per-paper setup:
```bash
uv init --name <project-name> --python 3.10
uv add <dependencies>
uv export --format requirements-txt --no-hashes -o requirements-kaggle.txt
```

## Kaggle CLI Execution Workflow
Authentication in this environment is stable with API token usage via environment variable.

One-command token usage pattern:
```bash
KAGGLE_API_TOKEN="<token>" kaggle competitions list
```

Notebook execution lifecycle:
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels push -p /abs/path/to/<paper_folder>
KAGGLE_API_TOKEN="<token>" kaggle kernels status <username>/<slug>
KAGGLE_API_TOKEN="<token>" kaggle kernels output <username>/<slug> -p /abs/path/to/<paper_folder>/outputs
```

## Operational Notes from Current Work
- Correct folder convention is root-level paper directories (not nested under an extra `kaggle/` container).
- Kernel title/slug consistency in `kernel-metadata.json` avoids push-time warnings.
- Kaggle runtimes can expose GPU/PyTorch capability mismatches; notebook code should include robust device fallback.
- Any file required at runtime must exist in the uploaded kernel folder.

## Current Implemented Paper
- `attention_is_all_you_need/`
  - Transformer implementation notebook
  - Kaggle kernel metadata
  - uv-managed dependency lock and export
  - downloaded training artifact and execution log
