# Dataset Generation Playbook v0.1 — Mathematics Pilot

Status: draft pilot artifact

This playbook defines how to turn the Mathematics pilot tree into small, auditable classification examples. It does not create an engine, a production dataset, a training run, or a model change.

## 1. Scope and source of truth

The pilot domain is Mathematics only. The two data artifacts are:

- `data/knowledge-tree-v0.1.json` — the parent/child view from domain to discipline, subdiscipline, and leaf.
- `data/atomic-disciplines-v0.1.json` — the derived leaf registry with explicit atomicity reasons and boundary tests.

The tree is a model-reconstructed perspective informed by MSC2020 reference areas. It is not a replacement for MSC2020 and it is not the canonical classification of Mathematics. The project rule remains: model-generated structures are hypotheses until reviewed.

The existing Knowledge-OS concept and relation schemas remain the field contract for identity, definition, provenance, confidence, and review state. The pilot files add only artifact-level contracts needed to audit a tree and its leaf decisions.

Reference: [MSC2020](https://msc2020.org/) and its [MathSciNet classification browser](https://mathscinet.ams.org/mathscinet/msc/msc2020.html).

## 2. Classification design

The pilot intentionally uses five broad disciplines:

| Discipline | Pilot subdisciplines |
|---|---|
| Algebra | Linear Algebra; Number Theory |
| Analysis | Calculus |
| Probability and Statistics | Probability; Statistics |
| Discrete Mathematics | Combinatorics; Graph Theory; Mathematical Logic |
| Geometry | Euclidean Geometry; Analytic Geometry; Differential Geometry |

The requested fields are therefore present without adding unrelated branches. Number Theory is placed under Algebra, and Probability/Statistics are grouped at the discipline level only to keep the pilot compact; both placements are explicit review items. A later graph view may add crosswalks or alternative parents without mutating this tree snapshot.

## 3. Atomicity contract

An atomic discipline is a useful leaf for classification, review, and a small behavior-focused dataset. It is not a claim that the field is indivisible. Each leaf must have:

1. a coherent scope;
2. a stable boundary against its siblings and parent;
3. an independent review target;
4. at least one traceable classification locator;
5. a confidence value in `[0, 1]`;
6. a `review_state` that distinguishes machine validation from human approval.

Do not add a leaf merely because it is a famous term. Reject or defer it when it is only a single theorem, a tool, a method with no stable field boundary, an application outside Mathematics, or a duplicate of an existing sibling.

## 4. Generation workflow

```text
tree nodes + relations
        |
        v
atomic leaf registry
        |
        v
candidate classification records
        |
        v
schema and invariant checks
        |
        v
deduplication and boundary review
        |
        v
small reviewed dataset candidate
```

### Step A — Load and normalize

Read the tree artifact as UTF-8 JSON. Index nodes by `concept_id`, relations by `relation_id`, and atomic records by `concept_id`. Do not infer a parent from a name or filename. Use `parent_ids` and the explicit tree relations.

Normalize only for comparison:

- trim surrounding whitespace;
- preserve original English and Arabic labels;
- compare identifiers case-sensitively;
- use Unicode-aware comparison for duplicate labels;
- do not silently rewrite source labels or provenance.

### Step B — Generate candidate records

Start with one behavior: `classify_node`. A minimal candidate record should contain:

```json
{
  "record_id": "math-classify-0001",
  "task": "classify_node",
  "input": "A question about eigenvalues and canonical forms of matrices.",
  "expected_node_id": "math.algebra.linear-algebra.matrix-theory",
  "expected_level": "atomic_discipline",
  "source_node_ids": ["math.algebra.linear-algebra.matrix-theory"],
  "dataset_version": "mathematics-classify-v0.1.0",
  "review_state": "unreviewed",
  "provenance": [{"source_type": "other", "source_id": "internally_authored_candidate", "locator": null}]
}
```

Keep candidates small and inspectable. Do not generate thousands of paraphrases. Every candidate must test a boundary, a parent/child decision, or a sibling distinction. A candidate that cannot be traced to a node and its definition is excluded.

### Step C — Validate deterministic structure

Run the following checks before any human review:

1. Parse both JSON files as UTF-8.
2. Confirm all required concept fields are present on every node/atomic record: `concept_id`, `name_en`, `concept_type`, `definition`, `status`, and non-empty `provenance`.
3. Confirm all required relation fields are present: `relation_id`, `source_id`, `relation_type`, `target_id`, `direction`, `status`, and non-empty `provenance`.
4. Confirm identifiers are unique within their artifact.
5. Confirm every `parent_id` exists and has exactly one matching tree relation.
6. Confirm the root has no parent, every other node has one parent, and the root reaches every node.
7. Confirm the graph is acyclic.
8. Confirm every relation source and target exists.
9. Confirm `level_index` partitions the node IDs exactly once.
10. Confirm every atomic record is a leaf in the tree and its parent is a subdiscipline.
11. Confirm confidence values are numeric and within `[0, 1]`.
12. Confirm review states use the project enum and that no machine check is reported as human approval.
13. Confirm every provenance entry has a source identity and every external source has an eligible locator/license decision.

The expected v0.1 counts are 45 nodes, 44 tree relations, and 28 atomic-discipline records. These numbers are an audit target, not permission to add padding records.

## 5. Review states and gates

Use the project review states as follows:

| State | Meaning in this pilot |
|---|---|
| `unreviewed` | Not yet passed deterministic checks. |
| `machine_validated` | JSON and internal invariants passed; no human endorsement implied. |
| `human_reviewed` | A named reviewer inspected the record and boundary. |
| `approved` | The record is approved for the explicitly named downstream use. |
| `rejected` | The record must not be used; preserve the reason. |

The current artifacts are machine-validated candidates. No record is human-reviewed or approved. Before a dataset release, a reviewer must inspect at least the five priority questions recorded in the tree artifact and the per-leaf `review_notes` in the atomic registry.

Recommended gate order:

1. structural checks;
2. provenance and license check;
3. duplicate and near-duplicate check;
4. boundary and placement review;
5. sample-level semantic review in English and Arabic;
6. approval of the dataset version;
7. only then, optional training or evaluation consumption.

## 6. Dataset sizing and splits

For the first candidate set, prefer one or two examples per atomic discipline plus boundary examples for each discipline. Keep a fixed, documented split seed and record exclusions. Do not create a large synthetic corpus before an evaluation demonstrates improvement.

The initial pilot must not mix `classify_node` with unrelated chat, tool-calling, or FunctionGemma records. If a later trainer requires conversational format, transform the small candidate set explicitly and validate the target chat template separately.

Use disjoint semantic cases across train and evaluation. Do not place a near-copy of an evaluation example in training. Include Arabic examples only after Unicode normalization and human language review; preserve the English node identifier as the stable target.

## 7. Provenance and release record

Every future dataset version must retain:

- source artifact paths and their versions;
- source locators and license/permission status;
- generation procedure version;
- deterministic validation report;
- deduplication and leakage results;
- train/validation/test counts and split seed;
- exclusions and unresolved review items;
- reviewer identity and approval state;
- runs that consumed the artifact, if any.

No dataset is publishable or training-eligible while provenance, licensing, duplication, privacy, or human approval gates remain unresolved.

## 8. Explicit non-goals for v0.1

- no classification engine;
- no graph database or API;
- no large dataset generation;
- no training, fine-tuning, or model-file changes;
- no mutation of existing schemas, decisions, prompts, or training documents;
- no claim of production readiness or official Mathematics coverage.

## 9. Pilot review queue

The structural review queue is 117 records: 45 tree nodes, 44 relations, and 28 atomicity records. All are `machine_validated`; zero are `human_reviewed` and zero are `approved`.

Priority decisions:

- Number Theory under Algebra versus a sibling discipline;
- grouping Probability and Statistics at discipline level;
- Mathematical Logic under Discrete Mathematics;
- cross-domain boundary of Graph Algorithms;
- method-oriented status of Bayesian Statistics;
- whether every leaf is independently useful for the intended classification behavior.
