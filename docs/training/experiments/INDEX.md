# Training Experiments Index

This directory contains durable, evidence-backed experiment records. Each experiment must separate:

- model identity;
- environment;
- dataset version;
- training configuration;
- evaluation methodology;
- raw failure evidence;
- final decision;
- unresolved risks;
- next gate.

## Experiments

| Experiment | Status | Model | Task | Result | Next gate |
|---|---|---|---|---|---|
| [Gemma 3 270M IT LoRA v0.1](./GEMMA-3-270M-IT-LORA-v0.1.md) | `PASS-AFTER-PARSER-FIX` | `google/gemma-3-270m-it` | Four-class bilingual ERP intent classification | Controlled validation/test rescored to 100% after isolated parser correction | Fully unseen 60-case challenge set |

## Public presentation assets

- [Standalone HTML case study](../public/gemma-3-270m-it-lora-v0.1.html)
- [Build in Public video brief](../public/VIDEO-BRIEF-GEMMA-270M-LORA-v0.1.md)

## Status interpretation

- **PREFLIGHT-PASS:** environment, data loading, forward/backward, adapter save/reload, and inference work.
- **MICRO-OVERFIT-PASS:** the selected model and pipeline can learn a tiny controlled set.
- **PASS-AFTER-PARSER-FIX:** preserved predictions pass after an evaluator/parser defect is corrected without retraining.
- **CHALLENGE-PASS:** a frozen adapter passes a newly authored unseen robustness set.
- **RELEASE-CANDIDATE:** technical, legal, safety, regression, and deployment gates pass for the declared scope.

A controlled 100% score is not automatically production readiness. Every public result must state the dataset scope and next unproven claim.
