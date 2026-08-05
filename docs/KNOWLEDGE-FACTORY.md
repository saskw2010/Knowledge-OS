# Knowledge Factory

**Status:** Proposed foundation addition.

## Purpose

The Knowledge Factory is the controlled pipeline that transforms source classification systems and evidence into validated graph content and derived datasets.

## Pipeline

```text
Collect
→ Register
→ Preserve
→ Normalize
→ Resolve
→ Expand
→ Validate
→ Publish Graph
→ Generate Dataset
→ Evaluate
```

## Stages

### 1. Collect

Acquire lawful, attributable source material and record its license, version, language, retrieval date, and integrity information.

### 2. Register

Create a classification-system record and define its purpose, family, identifiers, original structure, and import rules.

### 3. Preserve

Store an immutable representation of the original nodes, edges, labels, codes, and hierarchy. Never mix model-generated children into this layer.

### 4. Normalize

Normalize Unicode, language tags, identifiers, aliases, dates, and structural formats without changing source meaning.

### 5. Resolve

Detect possible duplicate or equivalent concepts and propose mappings to canonical concept nodes. Ambiguous matches remain unresolved until reviewed.

### 6. Expand

Run controlled node-expansion prompts to propose definitions, boundaries, children, prerequisites, applications, methods, skills, and cross-domain relations. Expansion output is always marked `model_inferred` until validated.

### 7. Validate

Compare proposals against official sources, independent models, domain experts, consistency rules, and graph constraints. Validation may approve, revise, reject, or preserve disagreement.

### 8. Publish Graph

Publish approved entities and relations with provenance, version, evidence, confidence, and status. Source snapshots remain separately queryable.

### 9. Generate Dataset

Create Q&A, explanations, flashcards, assessments, tool-use tasks, and evaluation items only from eligible graph claims. Each item retains a trace to the claims and evidence used.

### 10. Evaluate

Test factual support, coverage, duplication, leakage, bias, multilingual consistency, difficulty, and task suitability before training or release.

## Stop conditions for recursive expansion

A node must stop expanding when one or more conditions apply:

- configured maximum depth is reached;
- no authoritative distinction supports additional children;
- proposed children duplicate existing concepts;
- confidence falls below threshold;
- the node becomes an instance rather than a reusable concept;
- expansion adds negligible educational or analytical value;
- legal or provenance status blocks further processing;
- review capacity is insufficient for the generated volume.

## Audit requirement

Every stage emits a versioned event containing input identifiers, processor or model, prompt version, parameters, output identifiers, validation result, and timestamp. The factory must be reproducible and reversible, not a one-way content generator.
