# MK7 Routing Principles

Canonical doctrine: `MK7-HYBRID-INTELLIGENCE-DOCTRINE.md`.

```text
Request
  ↓
Hard Rules Gate
  ↓
Semantic Router
  ↓
Intent Classification
  ↓
Adapter Policy
  ├─ None
  ├─ ERP
  ├─ Arabic
  ├─ Tool-Use
  ├─ Format
  └─ Customer / Tenant
  ↓
Domain Runtime
  ↓
RAG / Concept Graph
  ↓
Tools / APIs / MCP
  ↓
Schema Validation
  ↓
Business Validation
  ↓
Audit Trace
```

## Rules
- Deterministic rules precede model reasoning when possible.
- Semantic routing should be cheap and low-latency.
- MK7 v1 uses single-adapter routing as the production baseline.
- Multi-adapter and Top-K composition are later phases.
- Validate hard Top-K routing before soft weighted blending.
- Verification includes schema, business rules, permissions, and audit checks.
- A General / No Adapter path must exist.
- Arabic adapters are used only when benchmarks show measurable improvement.

## MoLoRA evolution
```text
Enterprise Router → ERP Domain → ERP MoLoRA Router → Latent LoRA Experts
```
The enterprise router is inspectable and policy-driven. The MoLoRA router is learned and domain-internal.
