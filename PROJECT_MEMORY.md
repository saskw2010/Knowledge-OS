# Knowledge-OS Project Memory

This file is the compact operational state of the project. It does not replace specifications, KDRs, schemas, issues, or source documents. It points to the current accepted state and prevents agents and contributors from reconstructing project history from memory.

## Current phase

**Foundation and classification-system ingestion design**

The repository currently defines the project vision, foundations, classification-system strategy, schemas, prompts, execution plan, governance records, and initial model-generated seed view. Production ingestion code, graph storage, crosswalk automation, Dataset Factory, and model training remain future work.

## Accepted decisions

| ID | Title | Status | Target | Category |
|---|---|---:|---:|---|
| [KDR-0001](decisions/KDR-0001.md) | Canonical Concept as the shared semantic identity | Accepted | v0.1 | Ontology |
| [KDR-0002](decisions/KDR-0002.md) | Preserve multiple classification systems through crosswalks | Accepted | v0.1 | Architecture |

## Proposed decisions

| ID | Title | Status | Target | Category |
|---|---|---:|---:|---|
| [KDR-0003](decisions/KDR-0003.md) | Teacher-to-student model training pipeline | Proposed | v0.2+ | AI Training |

## Current architectural direction

```text
Classification systems
        ↓
Versioned classification nodes
        ↓
Canonical concept resolution
        ↓
Provenance-aware crosswalks
        ↓
Multi-perspective Meta-Knowledge Graph
        ↓
Validated node expansion
        ↓
Knowledge Factory
        ↓
Dataset Factory
        ↓
Evaluation and optional model training
```

## Current source-of-truth order

1. Accepted KDRs.
2. Versioned schemas and executable validation rules.
3. Official source-system specifications and licensed source data.
4. Current project specifications and architecture documents.
5. Validated graph records with provenance.
6. Proposed documents and open questions.
7. Brainstorm notes and model-generated hypotheses.

## Immediate work queue

1. Define measurable v0.1 acceptance criteria.
2. Select and document the first official classification-system MVP.
3. Complete the classification-node and mapping schemas.
4. Define provenance, claim, source, and validation-status contracts.
5. Import one official system without model-generated changes to its source hierarchy.
6. Create a crosswalk pilot between two systems.
7. Add automated schema, hierarchy, cycle, duplicate, and provenance tests.

## Open design areas

- Fundamental claim model beyond concepts and relations.
- Exact canonicalization and reversible merge policy.
- Confidence calculation and validation authority.
- RDF, SKOS, and OWL export or native usage.
- Graph and relational storage strategy.
- Multilingual identity and translation-equivalence policy.
- Dataset Factory contracts and evaluation design.
- Contributor governance and release gates.

See `docs/OPEN-QUESTIONS.md` for the maintained research and engineering question register.

## Working rules

- Do not silently alter accepted decisions.
- Add or update a KDR when architecture, ontology, research direction, or governance changes materially.
- Preserve original source classification structures.
- Keep extracted facts separate from inferred suggestions.
- Every imported relation must retain provenance, version, and validation state.
- Model outputs are candidates until validated.
- Brainstorm documents may add proposals but cannot override accepted KDRs.

## Last updated

2026-08-06
