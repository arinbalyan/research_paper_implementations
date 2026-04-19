# A Self-Improving Coding Agent (arXiv:2504.15228v2)

## 1) Objective
This project implements an MVP reproduction of **A Self-Improving Coding Agent** (arXiv:2504.15228v2), emphasizing the paper’s core claim: a coding agent can improve itself through an iterative, non-gradient update loop where code-level modifications are proposed, evaluated, and selectively retained.

The implementation is intentionally **Kaggle-first** for heavy execution, with local workflow limited to development, packaging, push, monitoring, and output retrieval.

## 2) Reproduction Scope
### Implemented in this MVP
- Iterative self-improvement loop over agent variants (`A_i` archive trajectory)
- Candidate proposal mechanism (simulated reflection/edit strategy)
- Utility-based selection balancing:
  - task accuracy
  - runtime latency
  - algorithmic complexity proxy
- Synthetic coding benchmarks for controlled, repeatable evaluation
- Full artifact export for reproducibility and post-run analysis

### Deferred to follow-up extensions
- Direct SWE-Bench-Verified subset integration
- LiveCodeBench integration
- External LLM-driven edit proposals and patch validation against real repositories

## 3) Method Overview
At iteration `i`, the incumbent agent is evaluated and compared against newly proposed variants. The best candidate is accepted only when utility improves.

Utility objective:

\[
U = w_a \cdot \text{Accuracy} - w_t \cdot \log(1 + \text{Latency}) - w_c \cdot \text{Complexity}
\]

where `w_a`, `w_t`, and `w_c` are configurable weights in the notebook runtime configuration.

This structure reproduces the paper-aligned non-gradient self-improvement pattern while keeping the benchmark environment deterministic and compute-efficient.

## 4) Benchmark Protocol (Synthetic Suite)
The MVP uses two synthetic coding benchmarks:

1. **Bug-fix task family**
   - Agent attempts to transform buggy snippets to corrected forms.
   - Controlled stochastic error model parameterized by `bug_rate`.

2. **Symbol retrieval task family**
   - Agent must locate target symbol definitions in synthetic files.
   - Controlled retrieval error model parameterized by `retrieval_error_rate`.

Each iteration evaluates multiple candidates over repeated trials (`EVAL_REPEATS`) and logs aggregate accuracy and latency statistics.

## 5) Project Structure
- `notebook.ipynb` — end-to-end implementation, evaluation loop, plots, and export
- `kernel-metadata.json` — Kaggle kernel configuration
- `pyproject.toml`, `uv.lock` — dependency specification + exact lock
- `requirements-kaggle.txt` — uv-exported dependencies for Kaggle runner parity
- `outputs/` — run artifacts and logs

## 6) Reproducibility and Dependency Workflow (`uv`)
Initialize and manage dependencies inside this paper folder only.

### Install / update dependencies
```bash
uv lock --project /home/nemesis/research_paper_implementations/self_improving_coding_agents
```

### Export Kaggle requirements
```bash
uv export --project /home/nemesis/research_paper_implementations/self_improving_coding_agents --format requirements-txt --no-hashes -o /home/nemesis/research_paper_implementations/self_improving_coding_agents/requirements-kaggle.txt
```

## 7) Kaggle CLI Run Lifecycle
Use token-based auth via `KAGGLE_API_TOKEN`.

### 1) Push kernel
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels push -p /home/nemesis/research_paper_implementations/self_improving_coding_agents
```

### 2) Monitor status
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels status arinbalyan/self-improving-coding-agents
```

### 3) Download outputs
```bash
KAGGLE_API_TOKEN="<token>" kaggle kernels output arinbalyan/self-improving-coding-agents -p /home/nemesis/research_paper_implementations/self_improving_coding_agents/outputs
```

## 8) Runtime Modes
Set runtime mode through environment variable:

- `RUN_MODE=smoke` (default): short validation run
- `RUN_MODE=paper_like`: longer multi-iteration run

## 9) Expected Artifacts
After execution, `outputs/` should contain:
- `iteration_metrics.csv` — tabular per-iteration accepted metrics
- `iteration_metrics.json` — archive-level structured trace
- `run_config.json` — reproducibility config snapshot
- `utility_curve.png` — trajectory plot
- `self_improving_coding_agents.log` — execution log

## 10) Result Interpretation Guidance
- Upward utility trend indicates successful iterative selection.
- Accuracy/latency trade-offs reveal whether improvements come from capability gains or runtime shortcuts.
- Complexity penalties discourage overfitting through uncontrolled growth of agent internals.

## 11) Threats to Validity / Limitations
- Synthetic benchmarks cannot capture full repository-scale engineering behavior.
- No direct API-coupled LLM self-edit loop in this MVP; proposal generation is simulator-driven.
- Reported paper headline numbers on SWE-Bench-Verified and LiveCodeBench are not claimed here.

## 12) Extension Roadmap
1. Plug in real code-edit proposal generation via external LLM API.
2. Add executable patch validation on repository snapshots.
3. Integrate SWE-Bench-Verified subset evaluation harness.
4. Integrate LiveCodeBench with strict protocol parity.
5. Add ablation studies over utility weights and proposal policies.
