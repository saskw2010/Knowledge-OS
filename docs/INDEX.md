# Knowledge-OS Documentation Index

This index is the navigation layer for the current Knowledge-OS foundation. It separates canonical decisions from research notes, prompts, schemas, data seeds, and external brainstorming material.

## Start / navigation

- [Repository Explorer](repository-map.html) — live visual map of the current GitHub repository tree with file-type filtering and direct actions.
- [Markdown Viewer](viewer.html) — renders repository Markdown as styled HTML without maintaining duplicate handwritten HTML files.
- [Repository Documentation Standard](REPOSITORY-DOCUMENTATION-STANDARD.md) — reusable WytSky/Sky365 standard for README, roadmaps, project memory, repository explorer, Markdown rendering, and visual documentation.
- [Master Roadmap](../MASTER-ROADMAP.md) — program-level map linking knowledge foundations, graph engineering, dataset production, model training, agentic-runtime research, and product delivery.
- [Manus–Gemini Import Workspace Index](MANUS-GEMINI-IMPORT-WORKSPACE-INDEX.md) — central task index for Gemini Import, Manus planning, source files, plans, decisions, and work links.
- [Gemini Import Workspace](../imports/gemini-brain/README.md) — organized workspace for raw sources, extracted knowledge, plans, decisions, and deliverables.

## Canonical foundation

- [Vision](VISION.md) — project purpose, boundaries, and epistemic position.
- [Architecture](ARCHITECTURE.md) — components, data flow, and graph layers.
- [Execution Plan](EXECUTION-PLAN.md) — classification systems → classification nodes → canonical concepts → graph → datasets.
- [Data and Legal Policy](DATA-AND-LEGAL-POLICY.md) — provenance, licensing, lawful acquisition, and dataset controls.
- [Core Roadmap](../ROADMAP.md) — staged delivery plan for the Knowledge-OS core implementation.

## Added foundation documents

- [Mission](MISSION.md) — operational mission and measurable outcomes.
- [Manifesto](MANIFESTO.md) — principles governing openness, traceability, plurality, and extensibility.
- [Foundations](FOUNDATIONS.md) — data, information, knowledge, understanding, wisdom, and meta-knowledge distinctions.
- [Classification Systems](CLASSIFICATION-SYSTEMS.md) — registry model for philosophical, educational, research, library, bibliometric, and model-generated systems, including multi-classification mappings from canonical concepts to many external nodes and paths.
- [Knowledge Factory](KNOWLEDGE-FACTORY.md) — ingestion, normalization, expansion, validation, graph publication, and dataset production.
- [Training Pipeline](TRAINING-PIPELINE.md) — Sky365 Tiny Training Lab landing page, knowledge-to-training flow, evaluation, and release gates.
- [Glossary](GLOSSARY.md) — canonical project terms.
- [Open Questions](OPEN-QUESTIONS.md) — unresolved research and engineering decisions.

## Sky365 Tiny Training Lab

- [Training Mind Map](training/TRAINING-MIND-MAP.md) — full map of the training system and decision gates.
- [Training Playbook](training/TRAINING-PLAYBOOK.md) — shortest safe route from discovery to verified release.
- [Current State](training/CURRENT-STATE.md) — single source of truth for the active model, environment, run history, blocker, and next action.
- [Environment Setup](training/ENVIRONMENT-SETUP.md) — multi-environment discovery, CUDA checks, and isolation rules.
- [Dataset Pipeline](training/DATASET-PIPELINE.md) — formats, validation, legal provenance, splits, and versioning.
- [Dataset Registry](training/DATASET-REGISTRY.md) — current local datasets, their task boundaries, and the one-adapter-per-task separation rule.
- [Experiment Lifecycle](training/EXPERIMENT-LIFECYCLE.md) — run manifests, logging, checkpoints, comparison, and failure preservation.
- [Evaluation Framework](training/EVALUATION-FRAMEWORK.md) — task, syntax, language, regression, and release evaluation.
- [Troubleshooting](training/TROUBLESHOOTING.md) — evidence-first diagnosis and recurring failure classes.
- [Lessons Learned](training/LESSONS-LEARNED.md) — durable corrections and reusable training knowledge.

### Active training prompts

- [Stage 3 — MK-7 Knowledge LoRA](training/prompts/STAGE-3-MK7-KNOWLEDGE-LORA-PROMPT.md) — train a new `sky365-gemma-mk7-knowledge-lora-v0.1` adapter on the Mustafian Knowledge dataset without mixing or modifying the ERP intent adapter.

## Visual / HTML documentation

- [Repository Explorer](repository-map.html) — live repository structure, search, filters, Markdown-vs-HTML distinction, and Agent Start Here.
- [MK7 v2 Learnable Multi-Adapter Runtime](architecture/MK7-V2-LEARNABLE-MULTI-ADAPTER-RUNTIME.html) — native visual architecture page.
- [Infographics Hub](infographics/index.html) — HTML hub for current visual engineering summaries.
- [Agent Swarm Orchestration](infographics/agent-swarm-orchestration.html) — delegation, isolation, monitoring, reports, governance, and decision gates for multi-agent execution.
- [Frontier Models Classification](infographics/frontier-models-classification.html) — research snapshot comparing GPT/Codex, Claude, Kimi, and GLM by task class rather than a single absolute ranking.
- [Training & Inference Roadmap](infographics/training-inference-roadmap.html) — local-vs-cloud map for LoRA, QLoRA, offloading, quantization, GGUF, hybrid inference, MoE-aware methods, and larger infrastructure.

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

## Rendering rule

Markdown remains the preferred source for canonical prose. HTML is reserved for interactive or visual pages, while `viewer.html` provides formatted HTML rendering for Markdown at view time. This avoids maintaining two divergent manual versions of the same document.
