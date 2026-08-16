# Coding GGUF / LM Studio Decision — 2026-08-13

Status: `PARTIAL`

## Scope

Bounded local inference comparison only. This is not training-readiness,
production acceptance, or evidence that GGUF weights can be fine-tuned.

## Storage integration

The three source GGUF files remain under `Q:\Colibri\models\coding`.
LM Studio imports use NTFS hard links under
`C:\Users\mosta\.lmstudio\models\colibri`, so no second model payload was
created. `Q:` free space remained 8.34 GiB after import.
The temporary discovery junction was moved outside the LM Studio scan root to
`Q:\Colibri\links\lmstudio-colibri-coding-junction`; exact model lookup is now
unambiguous.

| Model | GGUF size | LM Studio load | GPU estimate/observed | Functional | Strict instruction | Decision |
|---|---:|---|---:|---:|---:|---|
| Phi-4 Mini Instruct Q4_K_M | 2.321 GiB | VERIFIED | 2.32 GiB | 4/5 | 3/5 | Preferred local coding candidate |
| SmolLM3 3B Q4_K_M | 1.784 GiB | VERIFIED | 1.78 GiB | 4/5 | 0/5 | Secondary; verbose and Arabic normalization wrong |
| Granite 3.3 2B Instruct Q4_K_M | 1.439 GiB | VERIFIED | 1.44 GiB | 3/5 | 0/5 | Do not prioritize |

All runs used LM Studio on `127.0.0.1:1234`, context 2048, full GPU offload,
temperature 0, and the same five prompts. SmolLM3 required
`reasoning_effort=none`; its first run spent the response budget without
returning visible content.

## Evidence

- Harness: `training/harness/evaluate_lmstudio_coding_smoke.py`
- Phi: `training/evaluations/coding/phi-4-mini-q4-k-m-smoke-20260813/result.json`
- SmolLM3: `training/evaluations/coding/smollm3-q4-k-m-smoke-no-reasoning-20260813/result.json`
- Granite: `training/evaluations/coding/granite-q4-k-m-smoke-20260813/result.json`

## Decision

1. Use Phi-4 Mini first for local coding inference experiments.
2. Do not download trainable Phi weights yet; the five-case smoke is too small
   to justify a training track.
3. Do not use any of these models for MK7. The current MK7 winner remains the
   constrained discriminative classifier.
4. Before any coding fine-tune, build a separate, executable coding benchmark
   with isolated tests; never execute arbitrary generated code directly on the
   host.
5. Research Nemotron separately only after identifying the exact open-weight
   variant and proving that its quantized inference footprint fits the P2000.
