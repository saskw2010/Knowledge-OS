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
