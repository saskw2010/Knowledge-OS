# Sky365 Tiny Training Lab

> A reproducible operating system for moving from validated knowledge to a tested small-model release.

[![Status](https://img.shields.io/badge/status-active%20design-2563eb)](./training/CURRENT-STATE.md)
[![Target](https://img.shields.io/badge/target-Gemma%203%20270M%20IT-7c3aed)](./training/TRAINING-PLAYBOOK.md)
[![Method](https://img.shields.io/badge/default-LoRA-0f766e)](./training/TRAINING-MIND-MAP.md)
[![Policy](https://img.shields.io/badge/data-licensed%20%26%20traceable-92400e)](./DATA-AND-LEGAL-POLICY.md)

## Mission

Build a small, local, reproducible model-training pipeline that can prove one task end to end before expanding scope.

The immediate goal is not to add every optimization technique. It is to achieve one verified training success with:

- one clearly identified model;
- one clean dataset;
- one isolated environment;
- one repeatable command;
- one measurable evaluation;
- one preserved training record.

## Current operating model

| Layer | Purpose | Exit condition |
|---|---|---|
| **Infrastructure** | Prove the runtime, device, packages, paths, and model loading | A deterministic smoke test completes |
| **Training Pipeline** | Train one scoped task and preserve all evidence | A valid checkpoint or adapter passes evaluation |
| **Optimization** | Reduce memory, improve speed, or extend capability | Improvement is measured against the baseline |

> **Rule:** Optimization cannot replace proof. QLoRA, quantization, merging, routers, and MoE are introduced only after the baseline pipeline is verified.

## Training mind map

```mermaid
mindmap
  root((Sky365 Tiny Training))
    Objective
      Single task
      Success criteria
      Fixed evaluation set
      Release gate
    Infrastructure
      Hardware inventory
      Python environments
      PyTorch and CUDA
      WSL or native runtime
      Model and tokenizer loading
    Data
      Licensed sources
      Cleaning
      Schema validation
      Train validation test split
      Leakage and duplication checks
      Provenance
    Baseline
      CPU smoke test
      Few training steps
      Loss and checkpoint validation
      Before and after inference
    Training
      LoRA default
      Full fine tuning measured option
      QLoRA only when memory requires it
      Isolated run directory
      Reproducible seed
    Evaluation
      Known answers
      Unknown and refusal behavior
      JSON accuracy
      Function calling
      Arabic and English
      Regression tests
    Release
      Adapter or full model
      Optional merge
      Optional GGUF export
      Deployment test
      Model card and run record
    Learning System
      Failure registry
      Root cause analysis
      Lessons learned
      Next experiment
```

## Canonical flow

```mermaid
flowchart LR
    A[Validated graph claims] --> B[Dataset Factory]
    B --> C[Schema and provenance validation]
    C --> D[Train / Validation / Test]
    D --> E[CPU smoke test]
    E --> F{Smoke test passes?}
    F -- No --> G[Repair environment, data, or script]
    G --> E
    F -- Yes --> H[GPU LoRA baseline]
    H --> I[Independent evaluation]
    I --> J{Release gates pass?}
    J -- No --> K[Root-cause analysis]
    K --> B
    J -- Yes --> L[Versioned release]
```

## Knowledge-to-training flow

The original Knowledge OS controls remain mandatory:

```text
Validated Graph Claims
→ Dataset Factory
→ Teacher Review
→ Training/Evaluation Split
→ Student Training
→ Independent Evaluation
→ Release Gate
→ Monitoring and Feedback
```

### Teacher model

Generates, critiques, compares, and revises candidate learning items. Teacher approval is not sufficient by itself; outputs remain bound to source and graph validation.

### Dataset Factory

Converts eligible graph claims into task-specific records, deduplicates them, assigns difficulty and domain metadata, and preserves claim-level provenance.

### Student model

Learns from approved training records. Student behavior must be evaluated independently from the teacher used to generate the data.

## Required dataset partitions

- training;
- validation;
- held-out test;
- adversarial and contradiction set;
- provenance audit sample;
- multilingual consistency sample;
- domain-expert review sample.

## Release gates

A dataset or model release must pass:

1. schema validation;
2. source and license eligibility;
3. claim-to-evidence traceability;
4. duplicate and contamination checks;
5. unsupported-claim threshold;
6. bias and representation review;
7. multilingual terminology checks;
8. domain-specific safety checks;
9. independent benchmark evaluation;
10. documented human approval for the release scope.

## Non-circular evaluation rule

The same model configuration must not generate, validate, and judge the same records without independent controls. Use source verification, different models where appropriate, deterministic validators, held-out data, and human review.

## Feedback loop

Evaluation failures must update the graph or dataset lineage rather than being patched only in model prompts. The system should identify whether the root cause is source quality, concept mapping, relation error, generation prompt, dataset transformation, environment mismatch, training configuration, or model limitation.

## Training Lab navigation

| Page | Purpose |
|---|---|
| [Training Mind Map](./training/TRAINING-MIND-MAP.md) | Complete conceptual map and decision gates |
| [Training Playbook](./training/TRAINING-PLAYBOOK.md) | Repeatable route from an empty environment to a verified result |
| [Current State](./training/CURRENT-STATE.md) | Single source of truth for the active model, environment, run, blocker, and next action |
| [Environment Setup](./training/ENVIRONMENT-SETUP.md) | Runtime discovery and environment isolation rules |
| [Dataset Pipeline](./training/DATASET-PIPELINE.md) | Dataset lifecycle, formats, validation, and provenance |
| [Experiment Lifecycle](./training/EXPERIMENT-LIFECYCLE.md) | Run naming, logging, checkpoints, and comparison |
| [Evaluation Framework](./training/EVALUATION-FRAMEWORK.md) | Baselines, metrics, release gates, and regression tests |
| [Troubleshooting](./training/TROUBLESHOOTING.md) | Evidence-first diagnosis and common failure classes |
| [Lessons Learned](./training/LESSONS-LEARNED.md) | Durable record of discoveries, failures, and corrections |

## Immediate next action

Complete the environment and previous-run audit, then update [CURRENT-STATE.md](./training/CURRENT-STATE.md). No installation or new training should begin until the active Python environment, exact model identity, dataset, and last run are confirmed.
