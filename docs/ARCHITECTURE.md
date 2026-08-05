# Architecture

## 1. Architectural style

Knowledge-OS is a provenance-aware graph pipeline. Classification trees are imported as views; canonical concepts and semantic relations form the shared graph.

## 2. Core components

### Classification Registry
Stores classification systems, versions, authors, organizations, purposes, licenses, root categories, and original paths.

### Model View Elicitation
Runs a fixed prompt against multiple language models to produce independent Model-Reconstructed Human Knowledge Classifications.

### Ingestion Layer
Validates JSON, records provenance, preserves raw responses, and converts source-specific structures into a common staging model.

### Concept Normalizer
Normalizes Arabic and English names, aliases, identifiers, spelling variants, and concept types.

### Entity Resolution
Detects duplicates and near-equivalents without automatically collapsing disputed concepts.

### Crosswalk Engine
Creates mappings such as exact match, close match, broader match, narrower match, overlap, and no reliable match.

### Graph Store
Stores concepts, classification nodes, relations, perspectives, claims, evidence, confidence, versions, and review status.

### Recursive Expansion Orchestrator
Selects approved nodes, requests bounded expansion, detects cycles and duplication, and enforces depth and budget controls.

### Validation Engine
Combines schema validation, consistency checks, source verification, cross-model agreement, and human review.

### Dataset Generator
Produces Q&A, explanations, flashcards, exercises, classification tasks, graph reasoning tasks, and evaluation sets only from approved graph slices.

## 3. Logical data layers

```text
L0 Raw Sources
L1 Classification Systems
L2 Canonical Concepts
L3 Semantic Relations
L4 Claims and Evidence
L5 Perspectives and Crosswalks
L6 Learning Objects and Datasets
L7 Model Evaluation and Training Artifacts
```

## 4. Required entity types

- ClassificationSystem
- ClassificationVersion
- Perspective
- Concept
- ConceptAlias
- ClassificationAssignment
- Relation
- RelationType
- Claim
- Evidence
- Source
- ModelRun
- Review
- DatasetArtifact

## 5. Required controls

- Stable IDs independent of labels.
- Full provenance for generated and imported records.
- Confidence is not a substitute for evidence.
- No recursive expansion without depth, node, and cost limits.
- Cycle detection for hierarchical relations.
- Alternative mappings remain queryable.
- Every merge must be reversible.
- Model outputs remain immutable raw artifacts; corrections create reviewed derivatives.

## 6. Suggested implementation

Initial implementation can use Python, Pydantic, JSON Schema, and a graph adapter supporting Neo4j or PostgreSQL with Apache AGE. Storage choice should follow measured query needs rather than fashion.

## 7. Pipeline

```text
Register source
→ ingest raw classification
→ validate contract
→ normalize terms
→ propose canonical matches
→ review merge decisions
→ generate crosswalks
→ expand selected nodes
→ attach evidence
→ run validation
→ publish approved graph version
→ generate datasets
```
