# Knowledge-OS Foundations

**Status:** Proposed foundation addition.

## Epistemic ladder

Knowledge-OS distinguishes the following layers:

```text
Data
→ Information
→ Knowledge
→ Understanding
→ Wisdom
→ Meta-Knowledge
```

### Data

Recorded symbols, measurements, observations, statements, media, or events before interpretation in a defined context.

### Information

Data interpreted through labels, structure, context, or relationships so that it communicates something meaningful.

### Knowledge

Information represented as claims, concepts, procedures, rules, models, or justified relationships that can support explanation or action.

### Understanding

The ability to connect knowledge into causal, structural, historical, functional, or interpretive models and use those models appropriately.

### Wisdom

Context-sensitive judgment about goals, consequences, values, trade-offs, uncertainty, and responsible action. Wisdom cannot be inferred solely from graph connectivity or model confidence.

### Meta-Knowledge

Knowledge about knowledge: how a concept is classified, sourced, validated, disputed, taught, applied, versioned, and connected across different systems.

## Structural distinctions

Knowledge-OS must not collapse these entities:

- **Classification system:** a complete scheme created for a purpose.
- **Classification node:** a location within one named system.
- **Canonical concept:** an integration anchor across systems.
- **Claim:** a proposition associated with evidence and epistemic status.
- **Relation:** a typed connection between entities.
- **Source:** a document, dataset, institution, expert judgment, or model output that supports an entity or claim.
- **Perspective:** a rule or purpose that determines how knowledge is organized.
- **Dataset item:** a derived training or evaluation object, not original knowledge.

## Epistemic statuses

Every generated or imported assertion should use a controlled status such as:

- `source_extracted`
- `official_classification`
- `common_consensus`
- `model_inferred`
- `human_proposed`
- `disputed`
- `deprecated`
- `rejected`

Confidence scores do not replace these statuses. A high-confidence model inference remains an inference.

## Design consequence

The project does not seek a single final answer to “Where does this field belong?” It records answers of the form:

> Under system S, version V, for purpose P, concept C is classified at node N, supported by source E, with mapping confidence Q.

This statement form is the foundation of the Meta-Knowledge Graph.
