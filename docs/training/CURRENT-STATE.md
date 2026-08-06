# Current Training State

> This file is the single source of truth for the active Sky365 Tiny training effort. Update it after every meaningful discovery, run, failure, or decision.

**Last reviewed:** 2026-08-06  
**Status:** Controlled Gemma 3 270M IT LoRA v0.1 passed after an isolated parser correction; unseen robustness challenge is next.

## Active target

| Field | Current value | Confidence |
|---|---|---|
| Project | Sky365 Tiny local training pipeline | Confirmed |
| Canonical model | `google/gemma-3-270m-it` | Confirmed |
| Canonical local path | `Q:\Colibri\models\google\gemma-3-270m-it` | Confirmed |
| Architecture | `Gemma3ForCausalLM` | Confirmed |
| Model type | `gemma3_text` | Confirmed |
| Current task | Four-class Arabic/English/mixed ERP intent classification with JSON-only output | Confirmed |
| Training method | Ordinary PEFT LoRA | Confirmed |
| QLoRA | Out of current scope | Confirmed |
| FunctionGemma | Separate project; out of this experiment | Confirmed |
| Current experiment decision | `PASS-AFTER-PARSER-FIX` | Confirmed |
| Production readiness | Not established | Confirmed limitation |

## Verified active environment

| Component | Verified state |
|---|---|
| Environment | `Q:\Colibri\training\venv-py311` |
| Interpreter | `Q:\Colibri\training\venv-py311\Scripts\python.exe` |
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
| bitsandbytes | Not used or required for v0.1 |

## Dataset v0.1

| Item | State |
|---|---|
| Dataset | `sky365-gemma-intent-v0.1` |
| Train | 64 |
| Validation | 16 |
| Held-out test | 20 |
| Total | 100 |
| Classes | `employee.create`, `inventory.stock_query`, `sales.order_create`, `general.unknown` |
| UTF-8 / JSONL validation | PASS |
| Duplicate input check | PASS |
| Split-overlap check | PASS |
| Frozen held-out test | Preserved |

## LoRA configuration

```text
r = 8
alpha = 16
dropout = 0.05
QLoRA = false
```

Validated target modules:

```text
q_proj
k_proj
v_proj
o_proj
gate_proj
up_proj
down_proj
```

## Verified experiment history

### One-step preflight

| Metric | Result |
|---|---:|
| Trainable parameters | 1,898,496 |
| Total parameters | 269,996,672 |
| One-step loss | 7.618429183959961 |
| Peak allocated VRAM | Approximately 2,141 MB |
| Adapter save | PASS |
| Fresh-process reload | PASS |

### Micro-overfit gate

| Metric | Result |
|---|---:|
| Records | 8 — two per class |
| Steps | 50 |
| Initial loss | 5.782916 |
| Final loss | 0.000636 |
| Strict accuracy | 8/8 — 100% |
| Parsed accuracy | 8/8 — 100% |
| Valid JSON | 8/8 — 100% |
| Adapter reload | PASS |
| Peak allocated VRAM | Approximately 1.32 GB |

### Controlled LoRA run v0.1

| Metric | Result |
|---|---:|
| Epochs | 4 |
| Initial train loss | 0.299114 |
| Final train loss | 0.000063 |
| Peak allocated VRAM | Approximately 2.1 GB |
| Fresh-process adapter reload | PASS |
| Initial reported validation/test accuracy | 0% — invalidated by parser audit |

## Evaluator failure and correction

The controlled run initially appeared to fail because the model repeated a correct JSON object with `<end_of_turn>` until the generation limit. The original parser captured from the first opening brace to the last closing brace, combining multiple JSON objects into invalid JSON.

Parser fix v1:

1. stop at the first `<end_of_turn>` when present;
2. otherwise extract the first balanced JSON object;
3. parse and validate the first object only;
4. verify `intent` against the approved label set.

No model weights, adapter, dataset, or decoding configuration was changed during rescoring.

## Final verified controlled result

| Split | Parsed accuracy | Valid JSON | Wrong intents | Parser failures |
|---|---:|---:|---:|---:|
| Train audit sample | 12/12 — 100% | 12/12 | 0 | 0 |
| Validation | 16/16 — 100% | 16/16 | 0 | 0 |
| Held-out test | 20/20 — 100% | 20/20 | 0 | 0 |

Per-class accuracy and unknown handling were 100% on this controlled dataset.

Final decision:

```text
PASS-AFTER-PARSER-FIX
```

## Interpretation boundary

This experiment proves:

- the local environment is capable of Gemma 3 270M IT LoRA training;
- the model can learn the declared four-intent task;
- adapter save and fresh-process reload work;
- the held-out controlled set passed after correcting the evaluator;
- the apparent 0% result was a parser false negative.

It does not yet prove:

- production readiness;
- wide real-world Arabic/ERP robustness;
- multi-intent reliability;
- adversarial robustness;
- performance across many ERP modules and intents.

## Next action — one action only

Freeze the current base model, adapter, parser, dataset, and training configuration. Evaluate the existing adapter without new training on a fully unseen 60-case challenge set covering:

- new Arabic, English, and mixed-language wording;
- Egyptian dialect and realistic spelling noise;
- ambiguous requests;
- out-of-domain requests;
- multi-intent prompts;
- close class-boundary cases.

Proposed challenge thresholds:

```text
Valid JSON: 60/60
Overall parsed accuracy: at least 85%
Known-intent accuracy: at least 90%
Unknown handling: at least 80%
Parser failures: 0
```

## Documentation and public assets

- [Full methodology and experiment record](./experiments/GEMMA-3-270M-IT-LORA-v0.1.md)
- [Experiments index](./experiments/INDEX.md)
- [Standalone public HTML case study](./public/gemma-3-270m-it-lora-v0.1.html)
- [Build in Public video brief](./public/VIDEO-BRIEF-GEMMA-270M-LORA-v0.1.md)
- [Audit Snapshot Index — 2026-08-06](./audits/2026-08-06/INDEX.md)

## Local evidence path

```text
Q:\Colibri\training\runs\gemma-3-270m-it-lora-controlled-v0.1
```

## Change log

| Date | Change |
|---|---|
| 2026-08-06 | Created the single source of truth and recorded the correction that one Python environment cannot represent the whole machine. |
| 2026-08-06 | Incorporated the local audit, verified GPU environment, and Stage 2 gates. |
| 2026-08-06 | Locked `google/gemma-3-270m-it`, completed preflight and micro-overfit, ran controlled LoRA v0.1, isolated a parser-only false negative, and recorded `PASS-AFTER-PARSER-FIX`. |
