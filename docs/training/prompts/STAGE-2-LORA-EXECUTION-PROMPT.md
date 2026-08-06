# Stage 2 — Gemma 3 270M IT LoRA Execution Prompt

Use this prompt with Codex or the local coding agent **before starting Stage 2**.

```text
You are the execution engineer for Stage 2 of the Sky365 Tiny local-training program.

Your approved target is standard Gemma, not FunctionGemma.

Approved model identity

- Exact target repository: google/gemma-3-270m-it
- Model family: standard Gemma 3 text model
- Variant: instruction-tuned
- FunctionGemma is explicitly out of scope for this experiment
- Do not substitute google/functiongemma-270m-it
- Do not substitute google/gemma-3-270m base unless a separate owner decision approves continued pretraining

Repository and documentation context

- Knowledge repository: saskw2010/Knowledge-OS
- Canonical Training Lab: docs/TRAINING-PIPELINE.md
- Current state: docs/training/CURRENT-STATE.md
- Audit index: docs/training/audits/2026-08-06/INDEX.md
- Training playbook: docs/training/TRAINING-PLAYBOOK.md
- Dataset pipeline: docs/training/DATASET-PIPELINE.md
- Evaluation framework: docs/training/EVALUATION-FRAMEWORK.md
- Troubleshooting: docs/training/TROUBLESHOOTING.md

Verified local environment

- Active environment: Q:\Colibri\training\venv-py311
- Exact interpreter: Q:\Colibri\training\venv-py311\Scripts\python.exe
- Python: 3.11.9
- PyTorch: 2.6.0+cu124
- CUDA available: true
- GPU: Quadro P2000, 4GB VRAM
- Verified packages: transformers, datasets, accelerate, peft, trl, safetensors, sentencepiece
- bitsandbytes is not required for the first LoRA experiment

Current local-model fact

The previous audit found FunctionGemma assets, but did not confirm local weights for google/gemma-3-270m-it.

Therefore, before any dataset or training work:

1. Search all approved local model and Hugging Face cache locations for the exact standard Gemma model.
2. Confirm the candidate directory contains at minimum:
   - model.safetensors
   - config.json
   - tokenizer.json or tokenizer.model
   - tokenizer_config.json
   - chat_template.jinja
3. Read config.json and prove the identity matches google/gemma-3-270m-it.
4. Reject any directory whose config or README identifies FunctionGemma.
5. If exact weights are absent, stop with MODEL-WEIGHTS-MISSING and produce the precise gated-model download command. Do not silently train FunctionGemma instead.

Operating rules

1. Start in read-only discovery mode.
2. Do not use PATH Python.
3. Do not overwrite any prior dataset, run, checkpoint, adapter, model, or report.
4. Do not start QLoRA, MoE, model merging, GGUF export, or FunctionGemma work.
5. Do not start training until the experiment contract and dataset contract are approved.
6. Preserve commands, configs, seeds, logs, metrics, checkpoints, and evaluation outputs.
7. Never expose secrets or Hugging Face tokens.
8. Training completion is not semantic success.

Phase A — Verify exact model availability

Produce MODEL-AVAILABILITY-REPORT.md with:

- exact approved identity: google/gemma-3-270m-it
- all searched locations
- candidate local paths
- file inventory for each candidate
- config model_type
- architectures
- tokenizer class
- chat template presence
- safetensors presence and size
- final status: VERIFIED or MISSING or CONFLICT

Do not proceed unless status is VERIFIED.

Phase B — Lock the first task before creating data

The first local smoke task must test ordinary instruction following, not function calling.

Recommended task contract:

Convert short Arabic, English, and mixed-language instructions into concise, deterministic text or structured JSON responses for one narrow Sky365 behavior domain.

Before finalizing, create DATASET-DECISION.md comparing exactly three candidate first datasets:

A. Identity and boundary behavior
- model identity
- approved role
- known vs unknown behavior
- concise refusal when information is absent

B. Arabic ERP intent classification
- classify short Arabic and mixed-language requests into a fixed intent label set
- deterministic output schema

C. Simple structured extraction
- extract fixed fields from short Arabic and English requests into JSON
- no external tool call syntax

Score each candidate on:

- alignment with standard Gemma 3 270M IT
- deterministic evaluation
- dataset creation speed
- Arabic value
- risk of leakage
- suitability for 48-example smoke dataset
- usefulness to Sky365

Recommend one candidate only. Do not create the dataset until the recommendation and schema are written.

Phase C — Dataset contract

For the selected task, create EXPERIMENT-CONTRACT.md containing:

- experiment ID
- one-sentence task
- exact model identity and local path
- tokenizer and chat template
- input schema
- output schema
- label or field definitions
- train/validation/test counts
- baseline evaluation cases
- success criteria
- failure criteria
- stop conditions
- output directory
- LoRA method
- explicit exclusion of FunctionGemma and QLoRA

Default smoke-dataset envelope, subject to validation:

- total examples: 48
- train: 32
- validation: 8
- held-out test: 8

The test set must use unseen wording and must not be used to tune prompts or hyperparameters.

Phase D — Validate dataset

Create versioned files:

- train.jsonl
- validation.jsonl
- test.jsonl
- dataset-card.md
- schema.json
- validation-report.json
- duplicate-report.json
- leakage-report.json
- token-length-report.json

Validate:

- UTF-8 and JSONL correctness
- required roles and fields
- Gemma chat-template compatibility
- empty and malformed values
- duplicates and near-duplicates
- train/validation/test overlap
- answer leakage
- Arabic normalization
- output parse validity where structured
- token-length percentiles

Phase E — Frozen baseline

Evaluate untouched Gemma 3 270M IT on the full held-out test set before training.

Store:

- baseline-predictions.jsonl
- baseline-metrics.json
- baseline-summary.md

Use task-appropriate deterministic metrics. For structured output include parse success, schema validity, exact label or field accuracy. For text behavior include rubric-backed exact criteria and error categories.

Phase F — Discover LoRA targets from Gemma architecture

Inspect model.named_modules() and record all candidate linear modules.

Do not copy target modules from Llama, Qwen, or FunctionGemma assumptions without verifying the loaded standard Gemma architecture.

Create LORA-TARGET-REPORT.md with:

- candidate modules
- selected modules
- excluded modules
- trainable parameter count
- rationale

Phase G — One-step LoRA preflight

Create a new script. Do not patch the ByT5 script in place.

Use conservative hypotheses for the 4GB P2000:

- batch size: 1
- sequence length: 256 initially
- LoRA rank: 4 or 8 after parameter measurement
- alpha: 8 or 16
- dropout: 0.05
- bf16: false
- fp16: only after verified runtime test
- gradient checkpointing: use only if compatible and beneficial
- QLoRA: disabled

Perform:

1. dataset load
2. shortest/median/longest tokenization
3. forward pass
4. one backward pass
5. one optimizer step
6. verify only LoRA parameters are trainable
7. save adapter
8. close process
9. reload untouched base plus adapter in a fresh process
10. run one inference
11. record peak VRAM and RAM

Required output directory:

Q:\Colibri\training\runs\gemma3-270m-it-lora-smoke-v0.1-<timestamp>\

Required artifacts:

- MODEL-AVAILABILITY-REPORT.md
- DATASET-DECISION.md
- EXPERIMENT-CONTRACT.md
- environment.json
- model-identity.json
- dataset-manifest.json
- dataset-validation.json
- baseline-results.jsonl
- LORA-TARGET-REPORT.md
- lora-config.json
- preflight-results.json
- adapter\
- reload-test.json
- PREFLIGHT-REPORT.md

Execution boundary

Stop after adapter save and fresh-process reload. Do not start the multi-step controlled LoRA run in this pass.

Return exactly one decision:

- PREFLIGHT-PASS
- MODEL-WEIGHTS-MISSING
- PREFLIGHT-FAIL-DATA
- PREFLIGHT-FAIL-MODEL
- PREFLIGHT-FAIL-ENVIRONMENT
- PREFLIGHT-FAIL-CONFIG

Final response fields

Exact model identity:
Exact local model path:
Model availability status:
Selected dataset task:
Dataset rationale:
Dataset version and split:
Environment:
LoRA target modules:
Trainable parameters:
Preflight status:
Peak VRAM:
Adapter path:
Fresh-process reload result:
Primary blocker:
One recommended next action:
Artifacts created:
Knowledge-OS files proposed for update:
```
