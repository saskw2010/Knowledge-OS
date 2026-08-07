# Stage 3 — MK-7 Knowledge LoRA Training Prompt

Use this prompt with the local coding/training agent.

---

You are running the next controlled Sky365 Tiny experiment.

## Objective

Train a **new, separate LoRA adapter** on the Mustafian Knowledge dataset without mixing it with the ERP intent dataset or modifying the existing intent adapter.

Target adapter name:

```text
sky365-gemma-mk7-knowledge-lora-v0.1
```

Base model:

```text
Q:\Colibri\models\google\gemma-3-270m-it
```

Active Python environment:

```text
Q:\Colibri\training\venv-py311\Scripts\python.exe
```

Knowledge dataset:

```text
Q:\Colibri\training\datasets\sky365-mustafian-knowledge-v0.1-5-batches
```

Existing intent dataset — DO NOT USE:

```text
Q:\Colibri\training\datasets\sky365-gemma-intent-v0.1
```

Existing intent adapter — DO NOT MODIFY OR MERGE:

```text
sky365-gemma-intent-lora-v0.1
```

## Non-negotiable separation rule

This experiment is a knowledge-answering task, not an intent-routing task.

Do not:

- concatenate the intent dataset with the MK-7 dataset;
- continue training the intent adapter;
- merge the intent adapter into the base model;
- merge the two adapters;
- use the frozen 60-case intent Challenge Set as training data;
- rename the existing intent adapter;
- change the base model weights.

The required architecture is:

```text
google/gemma-3-270m-it
        |
        +-- sky365-gemma-intent-lora-v0.1
        |
        +-- sky365-gemma-mk7-knowledge-lora-v0.1
```

## Phase 0 — Read-only validation

Before training:

1. Verify the exact base-model identity from `config.json`.
2. Verify the active Python interpreter and CUDA visibility.
3. Validate all five dataset batches.
4. Confirm UTF-8 decoding.
5. Confirm JSONL validity.
6. Confirm no duplicate inputs.
7. Confirm no Train/Validation/Test overlap.
8. Confirm total expected volume:

```text
1000 total
800 train
100 validation
100 test
```

9. Read the dataset framework/owner-approval files if present.
10. Stop if the MK-7 canonical lens names are still explicitly marked unapproved by the owner.

Return a short PRETRAIN CHECK result before proceeding.

## Phase 1 — Experiment contract

Create a new isolated run directory, for example:

```text
Q:\Colibri\training\runs\gemma-3-270m-it-mk7-knowledge-lora-v0.1
```

Write `EXPERIMENT-CONTRACT.md` recording:

- exact base model;
- exact dataset path and manifest;
- task: knowledge Q&A / MK-7 canonical knowledge;
- adapter name;
- LoRA configuration;
- seed;
- precision used for training;
- evaluation rules;
- release thresholds;
- hardware.

Do not reuse the intent-run directory.

## Phase 2 — Baseline

Before training, run the untouched base model on a frozen sample covering:

- human-knowledge classification foundations;
- historical/library classification systems;
- exact MK-7 lens names;
- distinction between MK-7 and Dewey / taxonomy / knowledge graph;
- applied MK-7 reasoning;
- invalid or invented MK-7 lens names.

Save the raw outputs.

## Phase 3 — Micro-overfit gate

Before the full run:

1. Select a balanced 12–20 example micro-set.
2. Include examples from all five batches.
3. Run a small LoRA micro-overfit experiment.
4. Require the model to reproduce the correct answer patterns and exact canonical MK-7 names.
5. Save adapter and reload in a fresh process.

If the micro-overfit gate fails, do not start full training.

## Phase 4 — Controlled LoRA training

Use ordinary PEFT LoRA, not QLoRA unless measured VRAM pressure proves it necessary.

Start from the LoRA configuration that previously worked on Gemma 3 270M, unless dataset/task evidence requires one explicit change.

Preferred initial target modules:

```text
q_proj
k_proj
v_proj
o_proj
gate_proj
up_proj
down_proj
```

Preserve:

- deterministic seed;
- isolated checkpoints;
- train/validation history;
- best-adapter checkpoint;
- fresh-process reload test.

Do not change multiple major variables in the same run.

## Precision rule

The current Quadro P2000 has a verified inference issue:

```text
float16 inference -> all <pad>
float32 inference -> valid generation
```

Therefore:

- evaluation/inference on this P2000 must use `torch.float32` unless a new controlled test proves another dtype safe;
- training precision is a separate decision;
- do not blindly force FP32 training if the previously validated training configuration used another precision successfully;
- record training dtype and inference dtype separately.

## Phase 5 — Evaluation

Evaluate the new adapter in a fresh process using `torch.float32` inference on this machine.

Measure at minimum:

- non-empty generation rate;
- answer correctness;
- exact MK-7 canonical-name accuracy;
- invalid/invented lens-name rate;
- distinction accuracy between MK-7 and external classification systems;
- applied seven-lens coverage;
- Arabic quality;
- English/mixed-language behavior;
- fresh adapter reload consistency.

Do not score only formatting. Inspect semantic correctness.

## Critical anti-hallucination checks

The model must not invent official MK-7 lens names.

When asked whether a fake lens is canonical, it should reject it and use the approved seven names.

The model must also say that MK-7 is a project-specific framework, not a historical or globally accepted academic standard.

## Phase 6 — Independent unseen challenge

After training, create a **new challenge set not derived by simple paraphrase from the training templates**.

Do not train on it.

Include:

- direct MK-7 recall;
- paraphrased questions;
- cross-domain applications;
- adversarial fake lens names;
- comparisons with Dewey, taxonomy, ontology, and knowledge graphs;
- questions requiring multiple lenses;
- questions outside MK-7 scope;
- Arabic, English, and mixed language.

Freeze the challenge set before evaluating the adapter.

## Release rule

A successful training run does not replace the intent adapter.

If the knowledge adapter passes, preserve both independently:

```text
sky365-gemma-intent-lora-v0.1
sky365-gemma-mk7-knowledge-lora-v0.1
```

Routing between adapters is a later runtime/router experiment and is explicitly out of scope here.

## Final response format

Do not send long progress reports.

Return only:

```text
Pretrain validation:
Micro-overfit:
Full training:
Best validation result:
Fresh reload:
MK-7 exact-name accuracy:
General knowledge accuracy:
Invalid invented-lens rate:
Unseen challenge result:
Training dtype:
Inference dtype:
Peak VRAM:
Adapter path:
Decision:
One next action:
```

Allowed final decisions:

```text
PASS-KNOWLEDGE-ADAPTER
ITERATE-DATA
ITERATE-CONFIG
BLOCKED-ENVIRONMENT
CANONICAL-FRAMEWORK-NOT-APPROVED
```

---
