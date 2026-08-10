# Knowledge-OS

**Mustafa Multi-Perspective Knowledge Classification System**

Knowledge-OS is a research and engineering project for building a multi-perspective classification of human knowledge and representing it as a provenance-aware Meta-Knowledge Graph.

The project deliberately rejects the idea that all human knowledge must fit inside one universal tree. A single concept may appear in many classification systems, under different nodes and paths, depending on purpose, institution, discipline, historical context, or application.

## Start here

If you are a contributor, coding agent, or reviewer, read these files in this order:

1. [`README.md`](README.md) — orientation and repository map.
2. [`MASTER-ROADMAP.md`](MASTER-ROADMAP.md) — full program map across knowledge, graph, datasets, training, agents, and product layers.
3. [`PROJECT_MEMORY.md`](PROJECT_MEMORY.md) — current operational state and accepted decisions.
4. [`docs/INDEX.md`](docs/INDEX.md) — documentation navigation layer.
5. [`decisions/`](decisions/) — accepted and proposed Knowledge Decision Records (KDRs).
6. [`schemas/`](schemas/) — executable data contracts that code must obey.

## Core idea

Human knowledge should not be forced into one universal tree.

The same canonical concept may be projected into multiple classification systems:

```text
                           Canonical Concept
                                  │
          ┌───────────────────────┼───────────────────────┐
          ↓                       ↓                       ↓
 Classification System A  Classification System B  Classification System C
          │                       │                       │
       Node / Path A           Node / Path B           Node / Path C
```

Knowledge-OS therefore separates:

- **Canonical Concept** — the shared semantic identity.
- **Classification System** — the organizing perspective or source system.
- **Classification Node** — the concept's location inside a specific system.
- **Classification Mapping** — the explicit many-to-many link between canonical concepts and external nodes.
- **Canonical Relations** — graph relations that belong to Knowledge-OS itself.
- **Provenance / Evidence** — why a concept, mapping, or claim exists and where it came from.

This allows Knowledge-OS to preserve multiple perspectives without erasing disagreement or destroying source hierarchies.

## Architectural principles

1. One canonical concept may map to many nodes across many classification systems.
2. Canonical parentage must remain separate from external classification parentage.
3. Every classification system remains versioned and attributable.
4. Imported source structures remain immutable snapshots.
5. Trees are views; the underlying model is a graph.
6. Model-generated structures are hypotheses, not verified truth.
7. Provenance, confidence, validation state, and review state are first-class data.
8. Disagreement is preserved as alternative perspectives rather than silently merged away.
9. Training data must be generated only from controlled, versioned, and provenance-aware knowledge objects.
10. Brainstorm and research notes cannot silently override accepted KDRs or schemas.

## Program architecture

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

For the complete cross-project view, see [`MASTER-ROADMAP.md`](MASTER-ROADMAP.md).

## Repository structure

```text
Knowledge-OS/
│
├── README.md
│   └── Main orientation, architecture summary, and repository map.
│
├── MASTER-ROADMAP.md
│   └── Program-level roadmap covering the full Knowledge-OS ecosystem.
│
├── PROJECT_MEMORY.md
│   └── Compact operational state, accepted decisions, work queue, and rules.
│
├── ROADMAP.md
│   └── Core implementation roadmap for classification, graph, validation,
│       Dataset Factory, and product milestones.
│
├── index.html
│   └── GitHub Pages / visual project landing page.
│
├── decisions/
│   ├── KDR-0001.md
│   ├── KDR-0002.md
│   └── KDR-0003.md
│       └── Architecture and governance decision records.
│
├── docs/
│   ├── INDEX.md
│   │   └── Canonical documentation navigation layer.
│   │
│   ├── VISION.md
│   ├── MISSION.md
│   ├── MANIFESTO.md
│   ├── FOUNDATIONS.md
│   ├── ARCHITECTURE.md
│   ├── EXECUTION-PLAN.md
│   ├── CLASSIFICATION-SYSTEMS.md
│   ├── KNOWLEDGE-FACTORY.md
│   ├── TRAINING-PIPELINE.md
│   ├── DATA-AND-LEGAL-POLICY.md
│   ├── GLOSSARY.md
│   ├── OPEN-QUESTIONS.md
│   │   └── Core theory, architecture, execution, policy, and terminology.
│   │
│   ├── training/
│   │   ├── CURRENT-STATE.md
│   │   ├── TRAINING-MIND-MAP.md
│   │   ├── TRAINING-PLAYBOOK.md
│   │   ├── ENVIRONMENT-SETUP.md
│   │   ├── DATASET-PIPELINE.md
│   │   ├── DATASET-REGISTRY.md
│   │   ├── EXPERIMENT-LIFECYCLE.md
│   │   ├── EVALUATION-FRAMEWORK.md
│   │   ├── TROUBLESHOOTING.md
│   │   ├── LESSONS-LEARNED.md
│   │   └── prompts/
│   │       └── Training methodology, reproducibility, datasets,
│   │           evaluation, and active training prompts.
│   │
│   ├── architecture/
│   │   └── MK7-V2-LEARNABLE-MULTI-ADAPTER-RUNTIME.html
│   │       └── Model-runtime and learnable-router architecture notes.
│   │
│   ├── infographics/
│   │   ├── index.html
│   │   ├── agent-swarm-orchestration.html
│   │   ├── frontier-models-classification.html
│   │   └── training-inference-roadmap.html
│   │       └── Visual research summaries for orchestration, model comparison,
│   │           and local/cloud training and inference methods.
│   │
│   └── brainstorm/
│       └── Preserved external reviews, uploaded specifications, and ideas.
│           Brainstorm material is non-canonical unless promoted by a KDR.
│
├── schemas/
│   ├── classification-system.schema.json
│   ├── concept.schema.json
│   └── relation.schema.json
│       └── Versioned machine-readable data contracts.
│
├── prompts/
│   ├── root-human-knowledge-classification-ar.md
│   ├── expand-node-ar.md
│   └── validate-node-ar.md
│       └── Controlled prompts for root views, expansion, and validation.
│
└── data/
    └── seed/
        └── gpt56-root-view.sample.json
            └── Seed and future imported / generated data assets.
```

> The tree above is a navigation map, not a claim that every directory is equally canonical. Use `docs/INDEX.md`, KDR status, and the source-of-truth hierarchy below to determine authority.

## Source-of-truth hierarchy

When two files disagree, use this order:

1. Accepted KDRs.
2. Versioned schemas and executable validation rules.
3. Official source-system specifications and licensed source data.
4. Current canonical project specifications and architecture documents.
5. Validated graph records with provenance.
6. Proposed documents and research notes.
7. Brainstorm material and model-generated hypotheses.

## Main workstreams

### 1. Knowledge Foundation

Defines concepts, knowledge structures, classification systems, perspectives, and the rules that prevent premature collapse into one taxonomy.

### 2. Knowledge Graph Engine

Builds canonical concepts, classification mappings, crosswalks, provenance, evidence, validation state, and reversible graph operations.

### 3. Knowledge Factory

Ingests, normalizes, expands, validates, and publishes knowledge while preserving original source structures.

### 4. Dataset Factory

Converts approved graph slices into training, validation, test, and benchmark objects with contamination and provenance controls.

### 5. Model Factory / Training Lab

Experiments with small-model training, LoRA / QLoRA, local-vs-cloud constraints, reusable harnesses, multi-adapter systems, routing, and later larger models.

### 6. Agentic Runtime Research

Studies supervised subagent orchestration: delegation, isolation, monitoring, evidence collection, synthesis, and execution gates.

### 7. Product Layer

Targets graph browsing, search, perspective switching, concept lineage, source inspection, APIs, Knowledge Explorer, and training surfaces.

## Current status

The repository is still foundation-first. The core implementation priority remains classification contracts, canonical mapping, provenance-aware graph construction, validation, and reproducible ingestion before production-scale graph storage or automated Dataset Factory execution.

Training, multi-adapter, and agentic-runtime work are active research/engineering tracks, but they must not override the Knowledge-OS source-of-truth and data-governance rules.

For current operational state, always check [`PROJECT_MEMORY.md`](PROJECT_MEMORY.md).

## Documentation and visual pages

- [Documentation Index](docs/INDEX.md)
- [Master Roadmap](MASTER-ROADMAP.md)
- [Core Roadmap](ROADMAP.md)
- [Classification Systems](docs/CLASSIFICATION-SYSTEMS.md)
- [Training Pipeline](docs/TRAINING-PIPELINE.md)
- [Infographics Index](docs/infographics/index.html)

GitHub Pages project site:

`https://saskw2010.github.io/Knowledge-OS/`

## Working rule for coding agents

Before making architecture, graph, schema, ingestion, or training changes:

```text
README
  ↓
MASTER-ROADMAP
  ↓
PROJECT_MEMORY
  ↓
Accepted KDRs
  ↓
Relevant docs
  ↓
Relevant schemas
  ↓
Code / implementation
```

Do not reconstruct the project from chat history when the repository already contains an accepted source of truth.
