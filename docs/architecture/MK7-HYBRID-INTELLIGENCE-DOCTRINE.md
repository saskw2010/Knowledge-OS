# MK7 Hybrid Intelligence Doctrine

> **Enterprise-first intelligence: controlled at the top, adaptive at the bottom.**
>
> **ذكاء مؤسسي أولًا: تحكم في الأعلى، وتكيّف في الأسفل.**

## Core doctrine
Sky365 MK7 combines explicit controllable adapters with latent learned specialization.

### Explicit layer
- ERP LoRA
- Arabic LoRA
- Customer / Tenant LoRA
- Tool-Use / Format adapters
- Business-defined, auditable, tenant-aware

### Latent layer
- Domain-internal LoRA experts
- Learned MoLoRA router
- Specialization learned from training

**Both must live together. Explicit business routing remains above latent specialization.**

```text
MK7 Semantic Router
        ↓
Business / Domain Selection
        ↓
Domain MoLoRA Router
        ↓
LoRA 1  LoRA 2  LoRA 3  LoRA 4
      latent specialization
        ↓
Gemma Base Model
```

## Kimi / GLM comparison
Native MoE systems route among latent experts inside the model. MK7 adds an explicit enterprise control plane above learned specialization for auditability, tenant policy, permissions, and domain governance.

## Routing layers
1. Deterministic routing
2. Semantic routing
3. Adapter routing
4. Learned latent routing

## Roadmap
- MK7 v1: deterministic + semantic routing, single adapter per request
- MK7 v1.5: adapter registry, hot swap/cache, hard Top-K experiments
- MK7 v2: trainable domain-internal Mixture-of-LoRA router
- MK7 v2.5: soft composition with interference/stability testing
- MK7 v3+: native small MoE underneath MK7 while preserving enterprise routing

## Non-negotiable principles
- Route aggressively at every layer.
- Use the smallest capable model first; escalate only when necessary.
- Keep explicit business control above latent specialization.
- Use one canonical source of truth.
- Validate routing, adapters, permissions, schema, and business actions independently.
- Do not interpret LoRA blend coefficients as semantic percentages.
- Keep native MoE experts sparse/routed; do not collapse them into a dense average.

## Documentation rule
**Document once as doctrine, distribute many times as implementation guidance.**

This file is the canonical MK7 doctrine. Other repositories should link here and contain repo-specific implementation notes or summaries.
