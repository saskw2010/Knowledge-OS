# Knowledge-OS Documentation Index

This index is the navigation layer for the current Knowledge-OS foundation. It separates canonical decisions from research notes, prompts, schemas, data seeds, and external brainstorming material.

## Canonical foundation

- [Vision](VISION.md) — project purpose, boundaries, and epistemic position.
- [Architecture](ARCHITECTURE.md) — components, data flow, and graph layers.
- [Execution Plan](EXECUTION-PLAN.md) — classification systems → classification nodes → canonical concepts → graph → datasets.
- [Data and Legal Policy](DATA-AND-LEGAL-POLICY.md) — provenance, licensing, lawful acquisition, and dataset controls.
- [Roadmap](../ROADMAP.md) — staged delivery plan.

## Added foundation documents

- [Mission](MISSION.md) — operational mission and measurable outcomes.
- [Manifesto](MANIFESTO.md) — principles governing openness, traceability, plurality, and extensibility.
- [Foundations](FOUNDATIONS.md) — data, information, knowledge, understanding, wisdom, and meta-knowledge distinctions.
- [Classification Systems](CLASSIFICATION-SYSTEMS.md) — registry model for philosophical, educational, research, library, bibliometric, and model-generated systems.
- [Knowledge Factory](KNOWLEDGE-FACTORY.md) — ingestion, normalization, expansion, validation, graph publication, and dataset production.
- [Training Pipeline](TRAINING-PIPELINE.md) — teacher, dataset, student, evaluation, and release gates.
- [Glossary](GLOSSARY.md) — canonical project terms.
- [Open Questions](OPEN-QUESTIONS.md) — unresolved research and engineering decisions.

## Brainstorm and source review

- [Other-Agent Foundation Review v0.1](brainstorm/OTHER-AGENT-FOUNDATION-REVIEW-v0.1.md) — comparison of the uploaded foundation package with the current repository; additions only, no replacement of existing decisions.
- [Uploaded Master Specification v0.1](brainstorm/KNOWLEDGE-OS-MASTER-SPECIFICATION-v0.1.md) — preserved source note from the parallel ChatGPT-agent discussion.

## Schemas

- [`classification-system.schema.json`](../schemas/classification-system.schema.json)
- [`concept.schema.json`](../schemas/concept.schema.json)
- [`relation.schema.json`](../schemas/relation.schema.json)

## Prompts

- [`root-human-knowledge-classification-ar.md`](../prompts/root-human-knowledge-classification-ar.md)
- [`expand-node-ar.md`](../prompts/expand-node-ar.md)
- [`validate-node-ar.md`](../prompts/validate-node-ar.md)

## Seed data

- [`gpt56-root-view.sample.json`](../data/seed/gpt56-root-view.sample.json)

## Document status rules

- **Canonical:** approved project decision or contract.
- **Proposed:** additive design awaiting review.
- **Research:** source-backed analysis, not yet a project decision.
- **Brainstorm:** idea input preserved for comparison; cannot silently override canonical files.
- **Deprecated:** retained for provenance but not used as current guidance.
