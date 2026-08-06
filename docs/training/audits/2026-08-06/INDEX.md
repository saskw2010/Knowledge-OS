# Sky365 Tiny Training Audit — 2026-08-06

**Status:** Canonical audit snapshot index  
**Scope:** Local Colibri training environment, models, datasets, scripts, runs, blockers, and next-step readiness.

> This directory is a historical snapshot. Do not rewrite old findings to match later state. New discoveries should create a new dated audit or update the live state in `../../CURRENT-STATE.md`.

## Executive conclusion

The local machine is capable of small GPU training runs through:

```text
Q:\Colibri\training\venv-py311
Python 3.11.9
PyTorch 2.6.0+cu124
CUDA available: true
GPU: Quadro P2000 4GB
```

The environment has verified `transformers`, `datasets`, `accelerate`, `peft`, `trl`, `safetensors`, and `sentencepiece`. `bitsandbytes` is not verified and is not required for the first LoRA experiment.

The latest ByT5 experiment completed its GPU training loop and saved a checkpoint, but did **not** achieve the semantic task. Training completion must not be reported as model success.

## Audit documents

| No. | Document | Purpose | Key conclusion |
|---:|---|---|---|
| 00 | `00-executive-summary.md` | One-page handover | GPU environment verified; semantic learning still blocked |
| 01 | `01-project-tree.txt` | Local directory map | Colibri contains separate training, models, datasets, outputs, runs, and experiments |
| 02 | `02-environments.md` | Python, CUDA, WSL, Docker inventory | `venv-py311` is the only verified GPU training environment |
| 03 | `03-model-inventory.md` | Local model identity and format | FunctionGemma, ByT5, and mT5 assets must remain distinct |
| 04 | `04-dataset-inventory.md` | Dataset paths and quality notes | Current ByT5 dataset is far too small for generalization |
| 05 | `05-training-runs.md` | Previous training history | ByT5 run completed technically but failed semantically |
| 06 | `06-script-analysis.md` | Training script review | GPU loop works; device mismatch was fixed; eval design remains weak |
| 07 | `07-previous-vs-current.md` | State comparison | CUDA, PEFT, and TRL are now verified in the active environment |
| 08 | `08-errors-and-root-causes.md` | Failure registry | Dataset size, task alignment, and evaluation are the main causes |
| 09 | `09-readiness-matrix.md` | Component readiness | Environment is ready; dataset validation and evaluation remain partial/missing |
| 10 | `10-recommended-next-step.md` | Single recommended action | Prepare a controlled baseline before scaling |
| 11 | `machine-readable-report.json` | Machine-readable state | Source for automation and future dashboards |

## Verified local inventory

### Active environment

- `Q:\Colibri\training\venv-py311`
- Python `3.11.9`
- PyTorch `2.6.0+cu124`
- `torch.cuda.is_available() == True`
- GPU visible: `Quadro P2000`
- PEFT and TRL installed

### Other environments

- `Q:\Colibri\training\venv` — Python 3.13, CPU-only for Torch
- `Q:\Colibri\training\venv-py311-cu118` — incomplete and not approved for use
- System PATH Python must not be treated as the active training environment

### Models found

- `Q:\Colibri\models\functiongemma-270m-it`
- `Q:\Colibri\models\functiongemma-270m-it-training`
- `Q:\Colibri\models\byt5-small`
- `Q:\Colibri\models\mt5-small`

Important identity rule:

```text
google/gemma-3-270m
google/gemma-3-270m-it
google/functiongemma-270m-it
```

are separate model identities and must never be reported as interchangeable.

### Latest relevant run

```text
Script: Q:\Colibri\training\run_byt5_poc.py
Output: Q:\Colibri\training\outputs\byt5-poc-known-facts
Method: full-model Trainer run
Device: GPU
Technical status: completed
Task status: failed semantic success criteria
```

## Readiness summary

| Component | Status |
|---|---|
| Python environment | READY |
| PyTorch | READY |
| CUDA and GPU visibility | READY |
| PEFT | READY |
| TRL | READY |
| Model identity | PARTIAL — final LoRA target must be locked |
| Dataset | PARTIAL |
| Dataset validation | MISSING |
| Evaluation set | PARTIAL |
| LoRA script | PARTIAL |
| QLoRA support | PARTIAL and not currently required |
| Reproducibility | PARTIAL |
| Post-training evaluation | PARTIAL |

## Gate before Stage 2 LoRA

Stage 2 must not begin until all items below are confirmed:

- [ ] Exact target model path and model identity are locked.
- [ ] The task is described in one sentence.
- [ ] Output format is deterministic and documented.
- [ ] Train, validation, and held-out test files exist.
- [ ] Dataset schema validation passes.
- [ ] Duplicate and leakage checks pass.
- [ ] Baseline inference is stored before training.
- [ ] LoRA target modules are discovered from the loaded model architecture.
- [ ] A unique run directory is selected.
- [ ] Success and stop conditions are explicit.
- [ ] No QLoRA or `bitsandbytes` is introduced without measured VRAM need.

## Current recommendation

Do not continue the ByT5 POC as the primary proof of success. Use the verified GPU environment to run a controlled LoRA experiment on the explicitly selected Gemma/FunctionGemma model and a clean, task-aligned dataset, while preserving baseline, adapter, metrics, logs, and post-training evaluation.

## Related live documents

- [Current State](../../CURRENT-STATE.md)
- [Training Playbook](../../TRAINING-PLAYBOOK.md)
- [Dataset Pipeline](../../DATASET-PIPELINE.md)
- [Evaluation Framework](../../EVALUATION-FRAMEWORK.md)
- [Troubleshooting](../../TROUBLESHOOTING.md)
- [Stage 2 LoRA Execution Prompt](../../prompts/STAGE-2-LORA-EXECUTION-PROMPT.md)
