# Gemma 3 270M IT — Local LoRA Experiment v0.1

> **Status:** `PASS-AFTER-PARSER-FIX`  
> **Date:** 2026-08-06  
> **Project:** Sky365 Tiny / Knowledge-OS Training Lab  
> **Build in Public stage:** First reproducible local LoRA success on the approved 270M instruction model

## Executive result

A controlled LoRA experiment was completed locally on `google/gemma-3-270m-it` using an NVIDIA Quadro P2000 with 4 GB VRAM.

The training pipeline, dataset validation, adapter save/reload, and held-out evaluation all completed. The first reported `0%` validation/test result was later proven to be a parser false negative: the model generated the correct JSON intent, repeated before `<end_of_turn>`, while the original parser incorrectly captured from the first `{` to the last `}`.

After a single documented parser correction—extracting the first balanced JSON object or stopping at the first `<end_of_turn>`—the preserved predictions rescored to:

| Split | Parsed accuracy | Valid JSON | Wrong intents | Parser failures |
|---|---:|---:|---:|---:|
| Train audit sample | 12/12 — 100% | 12/12 | 0 | 0 |
| Validation | 16/16 — 100% | 16/16 | 0 | 0 |
| Held-out test | 20/20 — 100% | 20/20 | 0 | 0 |

All four intent classes, including `general.unknown`, scored 100% on the current controlled dataset.

> This proves the pipeline and the scoped experiment. It does **not** yet prove production robustness. A new unseen challenge set remains the next gate.

---

## 1. Objective

The experiment asked one narrow question:

> Can a very small instruction-tuned language model be adapted locally, using LoRA and a 4 GB professional GPU, to classify short Arabic, English, and mixed-language ERP requests into a fixed Sky365 intent and emit JSON only?

Approved intents:

```text
employee.create
inventory.stock_query
sales.order_create
general.unknown
```

Output contract:

```json
{"intent":"<approved-intent>"}
```

## 2. Canonical model identity

| Field | Value |
|---|---|
| Repository identity | `google/gemma-3-270m-it` |
| Canonical local path | `Q:\Colibri\models\google\gemma-3-270m-it` |
| Architecture | `Gemma3ForCausalLM` |
| Model type | `gemma3_text` |
| Model category | Standard Gemma 3 270M Instruction-Tuned |
| Weight format | `model.safetensors` |
| FunctionGemma | Explicitly out of scope |
| QLoRA | Explicitly out of scope |

Verified local files included:

```text
model.safetensors
config.json
tokenizer.json
tokenizer.model
tokenizer_config.json
chat_template.jinja
```

## 3. Hardware and software environment

| Component | Verified value |
|---|---|
| Operating system | Windows 11 Pro |
| CPU | Intel Core i7-8750H |
| RAM | Approximately 47.76 GB |
| GPU | NVIDIA Quadro P2000 |
| VRAM | 4 GB |
| Training environment | `Q:\Colibri\training\venv-py311` |
| Python | 3.11.9 |
| PyTorch | `2.6.0+cu124` |
| CUDA visible to PyTorch | `True` |
| Transformers | 4.57.6 |
| Datasets | 5.0.1 |
| Accelerate | 1.14.0 |
| PEFT | 0.20.0 |
| TRL | 1.9.2 |

Canonical interpreter:

```text
Q:\Colibri\training\venv-py311\Scripts\python.exe
```

The system Python in `PATH` was deliberately not treated as the training environment.

## 4. Dataset v0.1

Dataset identity:

```text
sky365-gemma-intent-v0.1
```

| Split | Records |
|---|---:|
| Train | 64 |
| Validation | 16 |
| Held-out test | 20 |
| Total | 100 |

The source design contained 25 examples per intent and included:

- Modern Standard Arabic;
- simple Egyptian Arabic;
- English;
- mixed Arabic/English ERP wording;
- direct and indirect requests;
- realistic minor spelling variation;
- out-of-domain cases mapped to `general.unknown`.

Validation gates passed:

- UTF-8 and JSONL validity;
- required fields present;
- no duplicate inputs;
- no overlap across train/validation/test;
- held-out test preserved;
- valid intent labels;
- model-compatible chat formatting.

## 5. Methodology

The work was divided into explicit gates rather than one long training command.

```mermaid
flowchart LR
    A[Model identity] --> B[Environment verification]
    B --> C[Dataset validation]
    C --> D[Frozen baseline]
    D --> E[One-step LoRA preflight]
    E --> F[8-example micro-overfit]
    F --> G[Controlled LoRA run]
    G --> H[Fresh-process evaluation]
    H --> I[Output/parser audit]
    I --> J[Rescore preserved predictions]
    J --> K[Challenge set — next]
```

### 5.1 Preflight

The one-step preflight verified:

- model and tokenizer load;
- CUDA execution;
- forward pass;
- backward pass;
- optimizer step;
- finite loss;
- adapter save;
- fresh-process adapter reload;
- inference after reload;
- VRAM measurement.

Selected LoRA target modules, discovered from the loaded model:

```text
q_proj
k_proj
v_proj
o_proj
gate_proj
up_proj
down_proj
```

Preflight measurements:

| Metric | Result |
|---|---:|
| Trainable parameters | 1,898,496 |
| Total parameters | 269,996,672 |
| One-step loss | 7.618429183959961 |
| Peak allocated VRAM | ~2,141 MB |
| Adapter reload | PASS |

### 5.2 Label and template audit

The audit confirmed:

- assistant response tokens were supervised;
- label masking was correct;
- padding was excluded from loss;
- the Gemma chat template was used consistently;
- training and inference template structure matched;
- the observed failure was not caused by label masking.

### 5.3 Micro-overfit gate

A temporary eight-example dataset was used to prove learnability before scaling.

| Metric | Result |
|---|---:|
| Examples | 8 — two per intent |
| Training steps | 50 |
| Initial loss | 5.782916 |
| Final loss | 0.000636 |
| Strict accuracy | 8/8 — 100% |
| Parsed accuracy | 8/8 — 100% |
| Valid JSON | 8/8 — 100% |
| Adapter fresh reload | PASS |
| Peak allocated VRAM | ~1.32 GB |

This established that the model, LoRA targets, labels, and adapter lifecycle could learn the task.

### 5.4 Controlled LoRA run v0.1

Configuration:

| Field | Value |
|---|---|
| LoRA rank | 8 |
| LoRA alpha | 16 |
| LoRA dropout | 0.05 |
| Dataset | 64 / 16 / 20 |
| Epochs completed | 4 |
| Stop condition | Early stopping after three evaluations without measured improvement |
| Initial train loss | 0.299114 |
| Final train loss | 0.000063 |
| Fresh-process reload | PASS |
| Peak allocated VRAM | ~2.1 GB |

The original evaluator reported validation and test accuracy of 0%. That result was not accepted blindly because it contradicted the successful micro-overfit and near-zero training loss.

## 6. Failure investigation

A train/validation/test inference audit was run in a new process with:

- the untouched base model;
- the selected best adapter;
- `add_generation_prompt=True`;
- deterministic decoding;
- `max_new_tokens=32`;
- correct slicing after the input token count;
- raw text, token IDs, EOS position, and parser status recorded.

Observed behavior:

- output was non-empty in 100% of cases;
- EOS was not emitted as the first token;
- adapter loading was verified;
- raw output began with the correct semantic JSON;
- the same JSON was repeated with `<end_of_turn>` until the token limit.

The original parser performed this invalid operation:

```text
first opening brace → last closing brace in the whole generated text
```

That combined multiple repeated JSON objects and produced invalid JSON.

### Parser fix v1

One isolated measurement correction was applied:

1. stop at the first `<end_of_turn>` when present;
2. extract the first balanced JSON object;
3. parse only that object;
4. validate that `intent` exists and belongs to the approved label set.

No model weights, dataset records, decoding parameters, or adapter files were changed during rescoring.

## 7. Final verified result

| Metric | Result |
|---|---:|
| Train audit sample parsed accuracy | 12/12 — 100% |
| Validation parsed accuracy | 16/16 — 100% |
| Held-out test parsed accuracy | 20/20 — 100% |
| Valid JSON after parser fix | 48/48 — 100% |
| Per-class accuracy | 100% |
| Unknown accuracy | 100% |
| Parser failures remaining | 0 |
| Wrong intents | 0 |
| Final decision | `PASS-AFTER-PARSER-FIX` |

## 8. What this experiment proves

The experiment supports these claims:

- Gemma 3 270M IT can be loaded and LoRA-trained locally in the verified environment.
- A Quadro P2000 with 4 GB VRAM was sufficient for this scoped LoRA experiment.
- The selected PEFT LoRA targets and label/template pipeline worked.
- The model learned the four-intent task on the controlled v0.1 dataset.
- The adapter could be saved and loaded in a fresh process.
- Raw-output auditing prevented a valid model from being incorrectly classified as a failed model.

## 9. What this experiment does not prove

It does not yet establish:

- production readiness;
- robustness to wide real-world ERP language;
- resistance to adversarial or highly ambiguous prompts;
- stable multi-intent behavior;
- performance across many modules or hundreds of intents;
- superiority over larger models;
- broad Arabic language competence.

The next required test is a fully unseen challenge set created after freezing the current model, adapter, parser, and dataset.

## 10. Key lessons

1. **Training loss is not a business metric.** It must be paired with held-out task evaluation.
2. **A 0% score may be an evaluator failure.** Inspect raw generations before changing the model or data.
3. **Micro-overfit is a powerful diagnostic.** If a model cannot memorize eight clean examples, scaling the dataset is premature.
4. **One controlled fix at a time preserves causality.** The parser was corrected without retraining.
5. **Fresh-process reload is mandatory.** It proves that the saved adapter—not hidden in-memory state—produces the result.
6. **Small local hardware can validate valuable ideas.** The correct scope matters more than headline parameter counts.
7. **Build in public should expose the debugging journey, not just the final score.** The false failure and its diagnosis are part of the value.

## 11. Next gate: unseen challenge set

Planned challenge set:

- 60 new examples created after the adapter freeze;
- new Arabic, English, and mixed-language wording;
- dialect and realistic spelling noise;
- ambiguous requests;
- out-of-domain requests;
- multiple-intent prompts;
- close boundary cases between classes.

Proposed release thresholds:

```text
Valid JSON: 60/60
Overall parsed accuracy: at least 85%
Known-intent accuracy: at least 90%
Unknown handling: at least 80%
Parser failures: 0
```

The challenge-set result will be appended as **Experiment v0.1 — Robustness Addendum**, preserving this document as the methodology and controlled-run record.

## 12. Local evidence paths

Primary local run:

```text
Q:\Colibri\training\runs\gemma-3-270m-it-lora-controlled-v0.1
```

Key evidence includes:

```text
FINAL-REPORT.md
artifacts\final-result.json
artifacts\training-history.json
evaluation\test-predictions.jsonl
inference-audit\TRAIN-VAL-TEST-INFERENCE-AUDIT.md
inference-audit\train-val-test-predictions.jsonl
inference-audit\generation-debug.json
inference-audit\parser-fix-v1.py
inference-audit\parser-fix-results.json
inference-audit\rescored-predictions.jsonl
inference-audit\PARSER-FIX-REPORT.md
```

The repository document records the methodology and results. Large weights, private machine paths, and local run artifacts are not automatically published.

## 13. Build in Public narrative

The business story is not merely “we achieved 100%.” The stronger story is:

> We built a reproducible local AI training lab, trained a 270M instruction model on a 4 GB GPU, initially saw an apparent total failure, refused to guess, traced the problem to the evaluator, corrected one parser defect without retraining, and recovered the model’s true 100% controlled-test result. The next public step is an unseen robustness challenge.

This narrative demonstrates:

- engineering discipline;
- cost-conscious local AI capability;
- transparent failure analysis;
- reproducibility;
- a path from research to Sky365 business modules;
- evidence-driven progress rather than marketing-only claims.
