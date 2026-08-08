# Sky365 MK-7 Training Package

This package documents the controlled Gemma 3 270M IT + LoRA experiment for Sky365 MK-7.

## Contents

- [Final report](FINAL-REPORT.md)
- [Interactive HTML report](FINAL-REPORT.html)
- [Training and adapter handover](TRAINING-EXPLAINED-HANDOVER.md)
- [Sky365 MK-7 infographic](sky365-mk7-infographic.png)

## Verified results

- Base: `google/gemma-3-270m-it`
- Runtime: FP32 on NVIDIA Quadro P2000
- Dataset: 800 train / 100 validation / 100 frozen test
- Controlled training: 6400 optimizer steps, 8 epochs
- Held-out MK-7 lens accuracy: 95% on the measured Test subset
- Fresh adapter reload: PASS
- Targeted MKU micro-overfit: 8/8 after first-complete-formula parsing

The full model weights and LoRA adapters remain local artifacts and are intentionally not committed to this documentation repository. Their local paths and provenance are recorded in the handover and final report.

## Scope boundary

This is training evidence and architecture documentation. It is not a claim of production deployment, MOE training, multi-adapter merge, or GGUF conversion.