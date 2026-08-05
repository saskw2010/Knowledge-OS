# Roadmap

## Phase 0 — Foundation

- [x] Define project vision and boundaries.
- [x] Define graph-first architecture.
- [x] Establish provenance and legal-data principles.
- [x] Create initial schemas and prompts.
- [ ] Approve naming and licensing strategy.

## Phase 1 — Classification Registry

- [ ] Implement common data contracts.
- [ ] Register philosophical and historical classification systems.
- [ ] Register ISCED-F, FORD, library, bibliometric, and professional systems with version metadata.
- [ ] Store original source paths and mappings without destructive normalization.

## Phase 2 — Multi-model root views

- [ ] Run the same root-classification prompt against selected teacher models.
- [ ] Store raw outputs and run metadata.
- [ ] Normalize concept labels and relation types.
- [ ] Measure agreement, omissions, and structural disagreement.

## Phase 3 — Canonical graph

- [ ] Build entity resolution workflow.
- [ ] Implement reversible merge decisions.
- [ ] Create crosswalk relations.
- [ ] Add confidence, evidence, and review state.
- [ ] Publish graph version 0.1.

## Phase 4 — Recursive node expansion

- [ ] Build bounded expansion prompt and orchestrator.
- [ ] Add cycle, duplication, and scope-leak detection.
- [ ] Prioritize nodes by coverage and value.
- [ ] Add human review queues.

## Phase 5 — Validation and evidence

- [ ] Add source-backed verification.
- [ ] Create multilingual review process.
- [ ] Audit cultural, academic, and language bias.
- [ ] Define acceptance thresholds by claim type.

## Phase 6 — Dataset factory

- [ ] Generate learning and evaluation objects from approved graph slices.
- [ ] Separate training, validation, and test data by provenance.
- [ ] Add contamination and duplication checks.
- [ ] Export JSONL and graph reasoning benchmarks.

## Phase 7 — Product layer

- [ ] Graph browser and search.
- [ ] Perspective switching.
- [ ] Concept lineage and source inspection.
- [ ] API for classification, expansion, mapping, and dataset generation.

## Near-term definition of done

Version 0.1 is complete when the repository contains validated schemas, three independent model root views, at least five registered external classification systems, a reviewed canonical root graph, and reproducible scripts for ingestion and validation.
