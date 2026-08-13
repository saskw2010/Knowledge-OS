# MK7 Architecture Reset — Decision

Status: `VERIFIED DIAGNOSTIC / IMPLEMENTATION DIRECTION APPROVED BY EVIDENCE`

## Decision

Stop treating the MK7 lens selector as a free-form JSON generation problem.
The accepted target architecture is:

`input -> out-of-scope/MKU boundary -> constrained multi-label lens classifier -> deterministic JSON contract -> optional LLM explanation`

The classifier selects canonical IDs. Code, not a language model, constructs
the output JSON. A generative LLM may explain the selected lenses but never
invent, add, or remove the route decision.

## Why this is a reset, not an optimizer tweak

The approved v0.3 data was held constant. The earlier Golden FP32 LoRA
checkpoint used free generation and scored Macro-F1 0.403 on the independent
60-case Challenge, with Unknown 0.000 and one invented lens name.

The new deterministic CPU baseline used the same dataset but transformed the
task into hashed character n-gram features plus a 9-output linear classifier:
seven canonical lenses, `__unknown__`, and `__mku__`. It code-constrains the
output schema.

| System | Macro-F1 | Exact set | Unknown | MKU | Invented lens |
|---|---:|---:|---:|---:|---:|
| Golden free-generation LoRA checkpoint 250 | 0.403 | 0.239 | 0.000 | 1.000 | 1 |
| Discriminative hash baseline v1 | 0.662 | 0.543 | 0.857 | 1.000 | 0 |

The baseline is still below acceptance gates (Macro-F1 >= 0.85; exact set >=
0.80; unknown >= 0.90). It is not a promoted production model. It does prove
that task representation and output constraint are major factors and should be
fixed before changing precision, GPU, or fine-tuning framework.

## Technology choice

### Now: deterministic baseline

- Python 3.11 + existing PyTorch only.
- CPU-compatible hashed multilingual character n-grams.
- Multi-label linear classification with separate unknown/MKU classes.
- Artifact: `training/harness/mk7_discriminative_hash_baseline.py`.

### Next: learned multilingual encoder classifier

After v0.4 data review, use a dedicated sequence-classification/embedding
model with sigmoid multi-label head. This is a new model-selection/download
decision and is not authorized by this document.

### Not now

- No QLoRA/FP16/Unsloth comparison: these optimize the failed generative
  architecture rather than the decision representation.
- No third environment: an online GPU only becomes useful after the classifier
  data contract is proven and model scale is the actual constraint.
- No Router default change: the current classifier is an evidence baseline.

## Remaining work

1. Create v0.4 review pack focused on unknown/support minimal pairs, complete
   multi-lens sets, and each low-recall lens.
2. Review and approve examples before training.
3. Retrain the constrained classifier and run the frozen Challenge.
4. Only after it passes: test a learned multilingual encoder classifier in the
   Training Lab; compare against the baseline with identical data/challenge.
5. Require human semantic review and owner decision before Router promotion.

## Evidence

- Baseline result: `Q:\Colibri\training\runs\mk7\discriminative-hash-v1-20260813\result.json`
- Baseline predictions: `Q:\Colibri\training\runs\mk7\discriminative-hash-v1-20260813\predictions.jsonl`
- Existing Golden result: `Q:\Colibri\training\evaluations\mk7\repair-v0.3-approved-step0250-challenge\result.json`
