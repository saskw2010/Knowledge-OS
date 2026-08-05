# Training Pipeline

**Status:** Proposed foundation addition.

## Flow

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

## Roles

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

Evaluation failures must update the graph or dataset lineage rather than being patched only in model prompts. The system should identify whether the root cause is source quality, concept mapping, relation error, generation prompt, dataset transformation, or model training.
