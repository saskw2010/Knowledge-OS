# Sky365 Tiny Dataset Registry

> Registry of training datasets and their intended adapters. Dataset roles must remain explicit to prevent accidental multi-task mixing.

## Active datasets

| Dataset | Local path | Task | Intended adapter | Status |
|---|---|---|---|---|
| `sky365-gemma-intent-v0.1` | `Q:\Colibri\training\datasets\sky365-gemma-intent-v0.1` | Four-class ERP intent classification | `sky365-gemma-intent-lora-v0.1` | Controlled training passed; robustness failed canonical intent-label control |
| `sky365-mustafian-knowledge-v0.1-5-batches` | `Q:\Colibri\training\datasets\sky365-mustafian-knowledge-v0.1-5-batches` | Human-knowledge classification + Mustafian Knowledge MK-7 Q&A | `sky365-gemma-mk7-knowledge-lora-v0.1` | Draft dataset; owner review required before canonical training |

## Separation rule

The two datasets are different tasks and must not be concatenated by default.

### Intent adapter

Purpose:

- deterministic ERP routing;
- exact canonical intent labels;
- JSON-only classification.

Current adapter:

```text
sky365-gemma-intent-lora-v0.1
```

### MK-7 knowledge adapter

Purpose:

- answer questions about human knowledge classification;
- compare classification systems;
- answer using the project-specific Mustafian Knowledge seven-lens framework;
- preserve the exact MK-7 canonical lens names after owner approval.

Planned adapter:

```text
sky365-gemma-mk7-knowledge-lora-v0.1
```

## Architectural rule

Use the same base model when appropriate, but keep separate LoRA adapters and separate evaluation suites.

```text
google/gemma-3-270m-it
        |
        +-- sky365-gemma-intent-lora-v0.1
        |
        +-- sky365-gemma-mk7-knowledge-lora-v0.1
```

Do not merge adapters or datasets until each task independently passes its frozen benchmark and a later multi-task experiment explicitly measures interference.

## Hardware/runtime note

On the current NVIDIA Quadro P2000, the validated inference path for the trained Gemma adapter uses `torch.float32`. `float16` produced all-`<pad>` generation in the Web inference wrapper. This is a verified local-runtime constraint, not a universal claim about Gemma or FP16.

Training precision is a separate decision: preserve the previously validated training configuration unless measured evidence requires changing it. Do not infer that inference must use the same dtype as training.

## MK-7 dataset package

The current package contains five batches:

1. human-knowledge classification foundations;
2. comparative classification systems;
3. MK-7 canonical framework;
4. applied MK-7 analysis;
5. robustness and canonical-name control.

Current generated volume:

```text
Total: 1000
Train: 800
Validation: 100
Test: 100
```

The package is marked `Draft / Owner Review Required` because the exact seven lens names and definitions must be approved before they become canonical model knowledge.
