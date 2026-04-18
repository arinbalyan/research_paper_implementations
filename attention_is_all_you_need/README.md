# Attention Is All You Need — Implementation Notes

## Objective
Implement a compact encoder-decoder Transformer ("Attention Is All You Need") with a reproducible, cloud-first execution workflow using Kaggle.

## Directory Contents
- `notebook.ipynb` — model implementation, training loop, evaluation sample, and artifact export
- `kernel-metadata.json` — Kaggle kernel configuration
- `pyproject.toml`, `uv.lock` — project dependency specification and lock
- `requirements-kaggle.txt` — exported requirements for external runners when needed
- `outputs/` — downloaded artifacts/logs from Kaggle runs

## Implementation Summary
The notebook includes:
- sinusoidal positional encoding
- multi-head attention
- position-wise feed-forward network
- stacked encoder and decoder layers with residual + layer normalization
- masked decoder self-attention
- sequence-to-sequence training on a synthetic reversal task
- greedy decoding example and model checkpoint export

## Execution Model
All substantial compute is delegated to Kaggle.
Local machine usage is limited to editing, packaging, pushing, and downloading outputs.

## Kaggle CLI Procedure
### 1) Push a run
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels push -p /home/nemesis/research_paper_implementations/attention_is_all_you_need
```

### 2) Monitor status
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels status arinbalyan/attention-is-all-you-need
```

### 3) Download outputs and logs
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels output arinbalyan/attention-is-all-you-need -p /home/nemesis/research_paper_implementations/attention_is_all_you_need/outputs
```

## Practical Learnings from This Implementation
- API token flow via `KAGGLE_API_TOKEN` is reliable for this environment.
- Absolute paths prevent shell path-splitting issues.
- Runtime dependency assumptions must match Kaggle upload contents.
- Hardware compatibility must be handled defensively in notebook runtime selection.

## Current Known Artifact
- `outputs/transformer_attention_is_all_you_need.pt`
- `outputs/attention-is-all-you-need.log`
