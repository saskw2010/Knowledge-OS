# Knowledge-OS Recursive Curriculum Engine v0.1

## Scope

This contract package defines the data boundary for the Mathematics V0.1 recursive curriculum engine. It covers knowledge nodes, atomic disciplines, curriculum ordering, the review-gated MK7 content view, and the content index.

It does not modify or define training data, a base model, an adapter, a trainer, or a model release. An MK7 view is content derived from reviewed knowledge; it is not an MK7 model artifact.

## Reference version

All five schemas use the exact reference value `Mathematics V0.1`. The canonical root for this slice is expected to be represented as a node such as `ko.math.v0.1.mathematics`; the schemas do not invent an official mathematical taxonomy. Existing classification systems remain attributable perspectives, and inferred children remain hypotheses until review.

## Files

| File | Contract |
|---|---|
| `schemas/knowledge-node.schema.json` | Versioned graph node with explicit parents, children, atomicity, review, and provenance. |
| `schemas/atomic-discipline.schema.json` | A reusable instructional unit that is atomic and has no child discipline. |
| `schemas/curriculum.schema.json` | Ordered stages over nodes and atomic disciplines, with curriculum-level gates. |
| `schemas/mk7-view.schema.json` | Source-linked, review-gated content view for MK7 consumption. |
| `schemas/content-index.schema.json` | Relative-path and SHA-256 index for content artifacts. |

## Common contract

Every object is versioned and carries a `review_state`. The allowed states are:

1. `unreviewed` — newly proposed or imported; not eligible for approval.
2. `machine_validated` — passed structural and deterministic checks only.
3. `human_reviewed` — a human inspected the declared scope and evidence.
4. `approved` — eligible for downstream composition under the relevant gates.
5. `rejected` — not eligible; preserve it for audit rather than silently deleting it.

`approved` is not implied by valid JSON. A state transition must preserve provenance and record the review decision outside these payload contracts. A rejected or unreviewed source cannot be treated as an approved curriculum claim.

## Parent-child rules

The schemas require explicit IDs and the same Mathematics V0.1 namespace, while the following are graph-level gates:

- A root node has `parent_ids: []`; every non-root node has at least one parent.
- A parent-child edge is symmetric: if A lists B in `child_ids`, B lists A in `parent_ids`.
- A node cannot be its own parent or child. The graph must be acyclic; `depth` increases by one along an edge when depth is supplied.
- A parent is broader than its child. Prerequisites, methods, applications, and cross-domain associations are not silently encoded as parent-child edges.
- Parent and child IDs are unique, belong to the same versioned graph, and resolve to existing nodes. Duplicate normalized canonical names at the same scope require a resolution decision.
- Multiple parents are allowed because Knowledge-OS is a graph, not one universal tree. A curriculum may choose one ordered path without erasing alternative parents.

## Atomicity rules

An `atomic-discipline` is the smallest reusable instructional unit for this slice. It must have at least one outcome and one core concept, support an independent assessment, and be reusable outside one example. Its `child_node_ids` must be empty, and its `atomicity.status` is exactly `atomic`.

A node is not atomic when it contains independently assessable subskills, requires separate outcomes, or has expandable children. A node may be `candidate_atomic` in the knowledge-node contract, but it cannot be promoted to an atomic discipline until the atomicity checks and human review pass. `atomic` and `approved` are separate facts: the first describes structure; the second describes governance.

## Recursive curriculum rules

Expansion is bounded and reversible. A stage references existing node and atomic-discipline IDs; it does not inline a second, conflicting ontology. Stop expansion when maximum depth is reached, no authoritative distinction exists, a duplicate is proposed, confidence is below threshold, the item becomes an instance, educational value is negligible, provenance is blocked, or review capacity is insufficient.

The curriculum `quality_gates` are assertions, not substitutes for the underlying checks. A curriculum is ready only when schema validity, symmetric acyclic parent-child links, atomicity, complete provenance, and declared review completion are all true.

## MK7 view rules

Each MK7 record must:

- cite at least one Mathematics V0.1 node and one atomic discipline;
- retain a non-empty parent path so the local item remains interpretable in the curriculum;
- originate from approved source claims or be explicitly marked for review;
- keep prompt and response traceable to claim/source IDs;
- pass duplicate and contamination/leakage checks before an approved view is released;
- preserve disagreement and reviewer notes instead of flattening uncertain claims;
- use independent review where the same model/process generated the candidate content.

The MK7 view schema deliberately contains no base-model ID, adapter path, training run, optimizer, checkpoint, or weight field. Those artifacts are outside this request and outside this knowledge contract. The content view must not be used as evidence that a model or adapter was trained or released.

## Quality-gate order

Use this order for a v0.1 slice:

1. Parse JSON and validate each payload against its schema.
2. Verify IDs, version namespaces, uniqueness, parent-child symmetry, acyclicity, and depth.
3. Verify provenance, license eligibility, and review-state eligibility.
4. Verify atomicity and curriculum stage prerequisites.
5. Verify MK7 source linkage, duplicates, and leakage boundaries.
6. Record the gate result and human decision; do not infer approval from a machine pass.

These files are contracts only. They do not claim that a Mathematics V0.1 graph, curriculum, MK7 view, or model release has already passed these gates.
