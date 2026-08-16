# Classification Systems Registry

**Status:** Proposed foundation addition.

## Purpose

This document defines how Knowledge-OS registers and preserves systems that organize human knowledge. It does not treat every listed item as the same type of system.

## System families

1. **Philosophical and civilizational systems** — Aristotle, Al-Farabi, Ibn Khaldun, Bacon, Comte, and other historically situated schemes.
2. **Educational systems** — classifications of programs, qualifications, curricula, and fields of study, such as ISCED-F.
3. **Research and development systems** — classifications used for research statistics and funding, such as OECD FORD.
4. **Library and documentary systems** — Dewey Decimal Classification, Library of Congress Classification, and related controlled vocabularies.
5. **Bibliometric and publication systems** — OpenAlex Topics, Scopus ASJC, Web of Science categories, and journal or paper taxonomies.
6. **Professional and competency systems** — occupations, work roles, tasks, skills, and competency frameworks.
7. **Semantic representation standards** — RDF, OWL, SKOS, and related technologies. These are representation languages or models, not classifications of human knowledge by themselves.
8. **Collaborative knowledge graphs** — Wikidata and similar resources. These are broad entity graphs, not equivalent to a single disciplinary taxonomy.
9. **Model-reconstructed systems** — GPT, Gemini, Claude, or other model-generated views, recorded as hypotheses rather than official standards.

## Core rule: one concept, many classification projections

Knowledge-OS must not force a canonical concept into a single classification tree or a single parent path.

A topic, discipline, skill, method, or other canonical concept may legitimately appear in multiple classification systems at the same time. Each system represents a different purpose, institutional context, historical lens, or operational use.

The canonical concept therefore remains independent from every source classification. Its positions inside external systems are stored as **classification projections** or mappings.

```text
Canonical Concept
├── classified_as → System A / Node X / Path A
├── classified_as → System B / Node Y / Path B
├── classified_as → System C / Node Z / Path C
└── classified_as → System D / Node W / Path D
```

Example: the same topic may be positioned differently in an educational taxonomy, a research taxonomy, a library taxonomy, a clinical taxonomy, and a model-reconstructed view. These positions must coexist; one projection must not overwrite another.

### Engineering implications

- `Canonical Concept` identity is separate from any classification node identifier.
- A concept may have zero, one, or many mappings per registered classification system.
- Each mapping must preserve the source system, source node, source path when available, source label, version, and provenance.
- Source hierarchy edges remain immutable snapshots of the originating system.
- Cross-system mappings are enrichment edges, not replacements for source hierarchy.
- `parent_ids` in the canonical concept graph must not be used as a shortcut for external classification parents.
- Code, APIs, graph queries, dataset generation, and visualization must assume **many-to-many concept ↔ classification-node relationships**.
- A view may ask: "How is this concept classified by each registered system?" and return all valid projections side by side.

This rule is foundational for future Knowledge Views and for any code that generates or traverses the multi-classification graph.

## Required registry fields

Each system record must include:

- system identifier;
- official and alternative names;
- system family;
- author or maintaining institution;
- purpose and intended use;
- historical and jurisdictional context;
- version and publication date;
- source location and retrieval date;
- license or permitted-use status;
- original hierarchy or graph structure;
- node identifier rules;
- language coverage;
- known limitations and biases;
- import and validation status.

## Preservation rule

Imported structures remain immutable source snapshots. Corrections, translations, mappings, and proposed children are stored in separate layers.

```text
Original source structure
├── source nodes
├── source edges
└── source labels

Knowledge-OS enrichment
├── normalized labels
├── canonical concept mappings
├── model-inferred relations
├── human-reviewed decisions
└── validation evidence
```

## Initial registry sequence

The recommended first implementation sequence is:

1. OECD FORD as a compact official research classification.
2. UNESCO ISCED-F as an educational comparison.
3. Dewey or Library of Congress as a documentary comparison.
4. OpenAlex Topics as a large machine-assisted research taxonomy.
5. One historical system.
6. GPT-5.6 model-reconstructed view.
7. Additional model views using the same prompt and controlled settings.

This sequence tests different purposes before scaling the graph.
