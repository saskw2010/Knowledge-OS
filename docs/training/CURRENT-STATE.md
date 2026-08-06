# Current Training State

> This file is the single source of truth for the active Sky365 Tiny training effort. Update it after every meaningful discovery, run, failure, or decision.

**Last reviewed:** 2026-08-06  
**Status:** Discovery required before the next training run

## Active target

| Field | Current value | Confidence |
|---|---|---|
| Project | Sky365 Tiny training pipeline | Confirmed |
| Intended model | Gemma 3 270M IT family | Partial |
| Local model discovered | `Q:\Colibri\models\functiongemma-270m-it-training` | Confirmed by audit |
| Exact model identity | Gemma 3 270M IT vs FunctionGemma 270M IT must be verified from config | Unresolved |
| Intended method | LoRA baseline after smoke test | Confirmed decision |
| QLoRA | Conditional only if measured memory pressure requires it | Confirmed decision |
| MoE | Out of immediate scope | Confirmed decision |
| Dataset | Must be identified from the existing project and previous runs | Unknown |
| Active Python environment | Not yet identified | Unknown |
| Last successful run | Not yet evidenced | Unknown |
| Last failed run | Not yet evidenced | Unknown |

## Known machine facts

| Component | Evidence-backed state |
|---|---|
| Operating system | Windows 11 Pro |
| CPU | Intel Core i7-8750H |
| RAM | Approximately 47.76 GB |
| GPU | NVIDIA Quadro P2000 |
| VRAM | 4 GB |
| NVIDIA driver | Detected and operational through `nvidia-smi` |
| WSL | WSL2 with Ubuntu distributions detected |

## Important correction

The initial audit examined `C:\Python313\python.exe`. It did not prove that PyTorch was absent from the entire machine. PyTorch may exist in another virtual environment, Conda environment, WSL distribution, Docker image, or project-local runtime.

Therefore, the following claim is invalid:

> PyTorch is not installed on the machine.

The evidence supports only:

> PyTorch was not detected in the specific Python 3.13 environment selected by the first audit.

## Primary blocker

The complete execution context is not yet known:

- actual Python environment used by previous training;
- exact model identity and source;
- dataset path and schema;
- scripts used for previous runs;
- run logs, checkpoints, and evaluation evidence.

## Next action

Run the read-only **Technical Discovery & Training Handover Audit** against the Colibri project and all relevant Windows, WSL, Conda, virtual-environment, and Docker contexts.

### Success criteria

The audit must produce evidence for:

1. exact active model;
2. exact active environment;
3. exact dataset;
4. previous run history;
5. last valid artifact;
6. one recommended next command.

## Change log

| Date | Change |
|---|---|
| 2026-08-06 | Created the single source of truth and recorded the environment-audit correction. |
