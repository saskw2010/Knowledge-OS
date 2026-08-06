# Current Training State

> This file is the single source of truth for the active Sky365 Tiny training effort. Update it after every meaningful discovery, run, failure, or decision.

**Last reviewed:** 2026-08-06  
**Status:** Audit complete; Stage 2 LoRA contract and dataset validation required

## Active target

| Field | Current value | Confidence |
|---|---|---|
| Project | Sky365 Tiny local training pipeline | Confirmed |
| Immediate objective | Complete one controlled, reproducible LoRA experiment with frozen baseline and held-out evaluation | Confirmed |
| Intended model family | Gemma 3 / FunctionGemma 270M instruction family | Partial |
| Strongest local training asset | `Q:\Colibri\models\functiongemma-270m-it-training` | Confirmed |
| Exact next model identity | Must be locked from `config.json`, tokenizer, template, and experiment contract before execution | Unresolved gate |
| Default training method | Ordinary PEFT LoRA | Confirmed |
| QLoRA | Not approved unless measured LoRA memory usage proves it necessary | Confirmed |
| Full fine-tuning | Not the default; may be measured later because the model is small | Deferred |
| MoE / model merging | Out of current scope | Confirmed |

## Verified active environment

| Component | Verified state |
|---|---|
| Environment | `Q:\Colibri\training\venv-py311` |
| Python | 3.11.9 |
| PyTorch | `2.6.0+cu124` |
| CUDA available to PyTorch | `True` |
| GPU | NVIDIA Quadro P2000 |
| VRAM | 4 GB |
| Transformers | 4.57.6 |
| Datasets | 5.0.1 |
| Accelerate | 1.14.0 |
| PEFT | 0.20.0 |
| TRL | 1.9.2 |
| Safetensors | 0.8.0 |
| SentencePiece | 0.2.2 |
| bitsandbytes | Missing / not required for first LoRA run |

### Environment rule

Use the exact interpreter:

```text
Q:\Colibri\training\venv-py311\Scripts\python.exe
```

Do not use the PATH Python or infer machine-wide package state from `C:\Python313\python.exe`.

## Verified previous work

### Latest ByT5 POC

| Field | State |
|---|---|
| Script | `Q:\Colibri\training\run_byt5_poc.py` |
| Model | `Q:\Colibri\models\byt5-small` |
| Dataset | `Q:\Colibri\training\datasets\byt5_poc_known_facts.jsonl` plus unknown example |
| Device | GPU |
| Technical result | Training loop completed and checkpoint saved |
| Task result | Semantic outputs remained incorrect / unstable |
| Conclusion | Runtime proof only; not a successful model-learning POC |

### Known prior tiny-model proof

A custom `llama2c` Sky365 Tiny experiment previously produced an exact known answer and an unknown-answer refusal on its narrow test, with a GGUF artifact. It is historical evidence, not the current Gemma/FunctionGemma LoRA baseline.

## Dataset state

| Item | Status |
|---|---|
| ByT5 four-row POC dataset | Too small; unsuitable as evidence of generalization |
| Sky365 QA behavior datasets | Present |
| Sky365 identity datasets | Present |
| Known/unknown POC dataset | Present |
| Train/validation/test contract for Stage 2 | Not yet locked |
| Dataset schema validation | Missing |
| Duplicate and leakage report | Missing |
| Frozen baseline evaluation set | Partial / must be formalized |

## Readiness matrix

| Component | Status |
|---|---|
| Active Python environment | READY |
| PyTorch and CUDA | READY |
| GPU visibility | READY |
| PEFT and TRL | READY |
| Local model files | READY / identity gate remains |
| Tokenizer and template | PARTIAL until selected model is locked |
| Dataset | PARTIAL |
| Dataset validation | MISSING |
| Baseline evaluation | PARTIAL |
| LoRA script | PARTIAL / new controlled script required |
| QLoRA support | PARTIAL and not required |
| Logging and reproducibility | PARTIAL |
| Post-training semantic evaluation | PARTIAL |

## Primary blocker

The machine and GPU environment are no longer the primary blocker.

The primary blocker is the **experiment contract**:

- exact model identity;
- one narrowly defined task;
- correct model-specific input/output template;
- clean versioned train/validation/test dataset;
- frozen task-level success criteria;
- validated LoRA target modules;
- isolated reproducible run directory.

## Next action — one action only

Execute the read-only and preflight sections of:

[Stage 2 — Controlled LoRA Execution Prompt](./prompts/STAGE-2-LORA-EXECUTION-PROMPT.md)

The agent must first produce `EXPERIMENT-CONTRACT.md`, validate the selected model and dataset, record the untouched baseline, and pass the one-step adapter save/reload preflight. It must not start the full LoRA run until those gates pass.

## Success criteria for Stage 2

Stage 2 is successful only when:

1. the exact base model and tokenizer are recorded;
2. the frozen baseline is stored;
3. a LoRA adapter is trained without modifying the base model;
4. the adapter reloads in a fresh process;
5. the same held-out evaluation is run before and after training;
6. task-level metrics improve to the declared threshold;
7. regressions and invalid outputs remain within the declared limits;
8. logs, config, adapter, metrics, and reproduction script are preserved;
9. one explicit decision is returned: PASS, ITERATE-DATA, ITERATE-CONFIG, CHANGE-MODEL, or BLOCKED-ENVIRONMENT.

Training completion or lower loss alone is not success.

## Audit references

- [Audit Snapshot Index — 2026-08-06](./audits/2026-08-06/INDEX.md)
- [Training Playbook](./TRAINING-PLAYBOOK.md)
- [Dataset Pipeline](./DATASET-PIPELINE.md)
- [Evaluation Framework](./EVALUATION-FRAMEWORK.md)
- [Troubleshooting](./TROUBLESHOOTING.md)

## Change log

| Date | Change |
|---|---|
| 2026-08-06 | Created the single source of truth and recorded the correction that one Python environment cannot represent the whole machine. |
| 2026-08-06 | Incorporated the full local audit: verified GPU environment, previous ByT5 result, model inventory, readiness, blockers, and Stage 2 LoRA gate. |
