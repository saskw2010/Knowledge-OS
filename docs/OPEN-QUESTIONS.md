# Open Questions

**Status:** Research registry. These questions are intentionally unresolved.

## Classification and ontology

1. What minimum ontology is required before importing the first official system?
2. When should two labels map to one canonical concept versus separate context-specific concepts?
3. How should historical concepts be mapped when their meanings do not match modern disciplines?
4. Which relation types are transitive, symmetric, inverse, or mutually exclusive?
5. How should polyhierarchy and cycles be represented and validated?

## Provenance and confidence

6. Should confidence be attached to nodes, claims, mappings, relations, or all four?
7. How should confidence from official sources, model agreement, and human review be combined without creating false precision?
8. What evidence is sufficient to promote `model_inferred` content to an approved state?

## Recursive expansion

9. What default depth and breadth limits should apply to each system family?
10. How should the factory distinguish a meaningful sub-concept from an example, instance, tool, skill, or application?
11. What novelty and duplication thresholds should stop expansion?

## Multilingual and civilizational representation

12. What is the canonical identifier strategy when no language provides a neutral preferred label?
13. How will Arabic, Islamic, African, Chinese, Indian, indigenous, and oral knowledge systems be represented from their internal conceptual frameworks?
14. How should translation disagreement be stored and reviewed?

## Dataset and training

15. What graph-validation state makes a claim eligible for dataset generation?
16. How will training and evaluation contamination be detected across generated variants?
17. Which tasks should be generated from hierarchy, crosswalk, causal, procedural, historical, and normative relations?
18. How will independent evaluation avoid circular teacher-model judgment?

## Governance and sustainability

19. Who can approve canonical mappings and releases?
20. How are disputes, appeals, deprecations, and version migrations governed?
21. Which components and datasets can be open, and which require restricted access due to licensing or safety?
22. How should the project measure coverage without rewarding uncontrolled graph growth?

## First implementation decision

The recommended pilot remains OECD FORD, followed by one educational system and one library or bibliometric system. Before ingestion begins, the project must finalize system, node, mapping, provenance, and validation schemas.
