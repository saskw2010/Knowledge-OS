# Knowledge-OS Master Roadmap

**Purpose:** one-page program map for contributors, coding agents, and reviewers. This file explains how the knowledge foundation, graph engine, dataset factory, model-training work, agentic runtime research, and product layer fit together.

> This is the program-level roadmap. `ROADMAP.md` remains the implementation roadmap for the Knowledge-OS core graph and classification work.

## 1. Program map

```text
Knowledge Foundation
        ↓
Classification Systems Registry
        ↓
Canonical Concepts + Multi-Classification Mappings
        ↓
Meta-Knowledge Graph
        ↓
Validation + Evidence + Provenance
        ↓
Knowledge Factory
        ↓
Dataset Factory
        ↓
Model Factory / Training Lab
        ↓
Agentic Runtime / Orchestration Research
        ↓
Product APIs + Graph Browser + Knowledge Explorer
```

## 2. Track A — Knowledge Foundation

Goal: define what knowledge objects are, how they are classified, and how competing perspectives are preserved.

Key areas:

- Vision, mission, manifesto, and foundations.
- Historical, educational, research, library, bibliometric, professional, and model-reconstructed classification systems.
- Canonical Concept as shared semantic identity.
- Multi-classification projection: one concept may map to many nodes and paths across many classification systems.
- Knowledge perspectives / views, including future formalization of the broader Knowledge Views matrix.
- MK-7 / Mustafian Knowledge research as a separate structured knowledge lens framework.

Primary references:

- `docs/VISION.md`
- `docs/FOUNDATIONS.md`
- `docs/CLASSIFICATION-SYSTEMS.md`
- `docs/GLOSSARY.md`
- `decisions/KDR-0001.md`
- `decisions/KDR-0002.md`

## 3. Track B — Knowledge Graph Engine

Goal: represent knowledge as a graph without collapsing all systems into one universal tree.

Core contracts:

```text
Classification System
        ↓
Classification Node
        ↘
         classification_mapping
        ↗
Canonical Concept
        ↓
Canonical relations / evidence / provenance
```

Engineering requirements:

- Many-to-many concept ↔ classification-node mappings.
- Preserve source hierarchy and version metadata.
- Keep canonical parentage separate from external classification parentage.
- Reversible entity resolution and merge decisions.
- Crosswalks between systems.
- Confidence, review state, and provenance on mappings and claims.
- Cycle, duplication, scope-leak, and hierarchy validation.

Primary references:

- `docs/ARCHITECTURE.md`
- `docs/EXECUTION-PLAN.md`
- `schemas/classification-system.schema.json`
- `schemas/concept.schema.json`
- `schemas/relation.schema.json`

## 4. Track C — Knowledge Factory

Goal: ingest, normalize, expand, validate, and publish structured knowledge while preserving evidence and source lineage.

Pipeline:

```text
Sources
  ↓
Immutable source snapshot
  ↓
Normalization
  ↓
Canonical mapping
  ↓
Expansion
  ↓
Validation
  ↓
Graph publication
```

Primary references:

- `docs/KNOWLEDGE-FACTORY.md`
- `docs/DATA-AND-LEGAL-POLICY.md`
- `prompts/`
- `data/`

## 5. Track D — Dataset Factory

Goal: convert approved graph slices and knowledge structures into reproducible learning and evaluation objects.

Requirements:

- Dataset provenance.
- Train / validation / test isolation.
- Duplicate and contamination checks.
- Task-specific dataset boundaries.
- Versioned JSONL exports.
- Evaluation objects and graph-reasoning benchmarks.

Primary references:

- `docs/training/DATASET-PIPELINE.md`
- `docs/training/DATASET-REGISTRY.md`
- `docs/training/EVALUATION-FRAMEWORK.md`

## 6. Track E — Model Factory / Training Lab

Goal: validate that structured Knowledge-OS outputs can teach small models and later scale to larger local or cloud models.

Current experimental sequence:

```text
Small local model
  ↓
Task-specific LoRA
  ↓
Verified reload + evaluation
  ↓
Reusable training harness
  ↓
Larger local model
  ↓
Multi-adapter / routing research
```

Research topics include:

- LoRA and QLoRA.
- CPU / RAM offload.
- Local-vs-cloud training boundaries.
- Multi-adapter systems.
- Learnable routing.
- MoE-aware and compressed-model training research.

Primary references:

- `docs/TRAINING-PIPELINE.md`
- `docs/training/`
- `docs/architecture/MK7-V2-LEARNABLE-MULTI-ADAPTER-RUNTIME.html`
- `docs/infographics/training-inference-roadmap.html`

## 7. Track F — Agentic Runtime Research

Goal: study how supervised multi-agent execution can operate above Knowledge-OS and the Model Factory.

Target control loop:

```text
Supervisor / Agent Manager
        ↓
Task decomposition
        ↓
Specialized subagents
        ↓
Isolation + monitoring
        ↓
Evidence and reports
        ↓
Synthesis
        ↓
Decision / approval gate
        ↓
Execution
```

Important distinction:

- Swarm scale = how many workers can operate in parallel.
- Orchestration = who assigns work, isolates responsibilities, monitors progress, aggregates reports, and decides whether execution may proceed.

Primary reference:

- `docs/infographics/agent-swarm-orchestration.html`

## 8. Track G — Product Layer

Goal: expose Knowledge-OS as usable infrastructure rather than only a research repository.

Planned capabilities:

- Graph browser.
- Search.
- Perspective switching.
- Concept lineage and source inspection.
- Classification and mapping APIs.
- Expansion and validation APIs.
- Dataset generation APIs.
- Knowledge Explorer.
- Training Lab surfaces.

Primary references:

- `ROADMAP.md`
- `docs/INDEX.md`
- `index.html`

## 9. Repository navigation contract

For a coding agent entering the repository for the first time:

1. Read `README.md` for orientation and repository structure.
2. Read `MASTER-ROADMAP.md` for the full program map.
3. Read `PROJECT_MEMORY.md` for the current accepted operational state.
4. Read accepted KDRs in `decisions/` before changing architecture.
5. Read `docs/INDEX.md` to locate detailed specifications.
6. Read the relevant schemas before writing graph or API code.
7. Treat brainstorm and research snapshots as non-canonical unless promoted by a decision record.

## 10. Source-of-truth hierarchy

1. Accepted KDRs.
2. Versioned schemas and executable validation rules.
3. Official source-system specifications and licensed source data.
4. Current canonical project specifications.
5. Validated graph records with provenance.
6. Proposed documents and research notes.
7. Brainstorm and model-generated hypotheses.

## 11. Near-term priority

The immediate engineering focus remains the Knowledge-OS core:

1. Complete classification-node and mapping contracts.
2. Import the first official classification system without destructive normalization.
3. Build a crosswalk pilot.
4. Add validation tests.
5. Publish the first reviewed canonical graph slice.
6. Feed approved graph slices into the Dataset Factory.
7. Reuse the Training Lab only after data contracts and evaluation boundaries are explicit.

---

**Rule:** Knowledge-OS is not one taxonomy, one model, or one tree. It is a provenance-aware system for preserving multiple knowledge perspectives, mapping them to canonical concepts, validating them, and turning approved knowledge into reusable graph, dataset, model, and product assets.
