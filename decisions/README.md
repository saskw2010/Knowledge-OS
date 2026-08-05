# Knowledge-OS Decision Records

This directory stores **KOS Decision Records (KDRs)**: durable research and engineering decisions for Knowledge-OS.

A KDR records not only the selected decision, but also its context, rationale, assumptions, alternatives, consequences, risks, dependencies, review date, and related evidence.

## Rules

1. Use `KDR-TEMPLATE.md` for every new decision.
2. Assign IDs sequentially: `KDR-0001`, `KDR-0002`, and so on.
3. Do not rewrite accepted history silently. Amend, supersede, deprecate, or reject through a new KDR or an explicit status change.
4. Proposed decisions must not be treated as implemented architecture.
5. Every accepted KDR must be linked from `PROJECT_MEMORY.md`.
6. Research claims and external standards must include references or be marked as assumptions.
7. Review dates are mandatory for decisions based on unstable technology, incomplete research, or temporary constraints.

## Initial register

| ID | Title | Status | Target | Category |
|---|---|---:|---:|---|
| KDR-0001 | Canonical Concept as the shared semantic identity | Accepted | v0.1 | Ontology |
| KDR-0002 | Preserve multiple classification systems through crosswalks | Accepted | v0.1 | Architecture |
| KDR-0003 | Teacher-to-student model training pipeline | Proposed | v0.2+ | AI Training |
