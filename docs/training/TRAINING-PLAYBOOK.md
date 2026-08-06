# Sky365 Tiny Training Playbook

This playbook defines the shortest safe route from an unknown machine state to a verified model-training result.

## Operating principle

Prove one task end to end before increasing model size, data volume, optimization complexity, or architectural scope.

## Phase 0 — Freeze the target

Record:

- exact task;
- exact input and output format;
- fixed evaluation examples;
- acceptance threshold;
- prohibited failure modes.

**Exit condition:** one-page task contract exists.

## Phase 1 — Discover the existing system

Inspect all relevant Windows, WSL, virtual-environment, Conda, Docker, model, dataset, script, log, run, and checkpoint locations.

Do not install or modify anything during discovery.

**Exit condition:** [CURRENT-STATE.md](./CURRENT-STATE.md) identifies the actual environment, model, dataset, last run, blocker, and next command.

## Phase 2 — Validate model and data loading

Verify:

- the model is a Transformers-compatible training source;
- tokenizer and config load;
- the chat template matches the model family;
- one dataset record can be parsed and tokenized;
- secrets and local paths are not embedded in committed artifacts.

**Exit condition:** one deterministic inference sample and one tokenized training sample succeed.

## Phase 3 — CPU smoke test

Run only enough steps to prove the pipeline:

- 2–20 steps;
- tiny dataset subset;
- finite loss;
- checkpoint or adapter save;
- reload test;
- before/after inference comparison.

CPU is used here for diagnosis and reproducibility, not for long production training.

**Exit condition:** a loadable artifact is produced and the run record is complete.

## Phase 4 — GPU LoRA baseline

LoRA is the default first GPU training method.

Required controls:

- isolated output directory;
- fixed seed;
- explicit model and dataset paths;
- conservative sequence length;
- batch size 1 initially;
- gradient accumulation if needed;
- train and validation metrics;
- GPU-memory observation;
- post-training reload and inference.

**Exit condition:** LoRA adapter passes the fixed evaluation set and improves the target task without unacceptable regression.

## Phase 5 — Compare alternatives only when measured

### Full fine-tuning

Evaluate it because a 270M model is small, but calculate memory and test it in isolation before adoption.

### QLoRA

Use only when ordinary LoRA cannot fit or when a larger model is introduced. QLoRA is not a mandatory maturity stage.

### Quantization

Treat primarily as an inference and deployment concern unless the training design explicitly uses quantized loading.

### Multi-model routing

Prefer an external router when separate specialists are easier to train, evaluate, replace, and govern.

### MoE

Keep outside the immediate path. Consider only after multiple useful specialists and a measured routing requirement exist.

## Phase 6 — Evaluation

A release candidate must be measured on:

- task correctness;
- JSON validity;
- tool or function selection;
- Arabic behavior;
- English behavior;
- unknown-input handling;
- refusal boundaries where applicable;
- regression against the original model;
- held-out examples not generated and judged by the same model configuration.

## Phase 7 — Release and preserve

Every accepted release should include:

- adapter or complete model artifact;
- tokenizer and configuration references;
- dataset version and lineage;
- training configuration;
- environment lock or package snapshot;
- metrics;
- failure notes;
- model card;
- exact reproduction command.

## Definition of success

A training run is not considered successful merely because it finished or because loss decreased.

It is successful only when:

1. the output artifact reloads;
2. the fixed evaluation improves or meets threshold;
3. the process is reproducible;
4. the result and evidence are versioned;
5. known regressions and limitations are documented.
