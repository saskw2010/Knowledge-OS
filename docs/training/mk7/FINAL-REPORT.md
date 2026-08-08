# MK-7 Controlled Training Final Report

- Run: clean-20260808-153551
- Base: google/gemma-3-270m-it
- Adapter: checkpoints/best-adapter
- Device: NVIDIA Quadro P2000 (CUDA)
- dtype: torch.float32
- Dataset: 800 train / 100 validation / 100 test
- Test policy: frozen during training and checkpoint selection
- Optimizer steps: 6400
- Epochs: 8
- Initial loss: 4.14940071105957
- Final loss: 0.0005319382762536407
- Best validation loss: 0.10316036139323842
- Runtime: 1459.26 seconds

## Fresh Reload Evaluation

| Split | Records | Non-empty | Exact full-text | MK-7 lens accuracy |
|---|---:|---:|---:|---:|
| Validation | 100 | 100/100 | 3/100 | 37/40 (92.5%) |
| Test | 100 | 100/100 | 3/100 | 38/40 (95.0%) |

Fresh reload: PASS.

The exact full-text metric is informational and is not an appropriate primary metric for generated knowledge answers: semantically correct answers may be shorter or differently worded. MK-7 lens accuracy is the relevant measured semantic proxy for the 40 MK-7 records in each split. The current evaluation set contained no explicit MKU records, so MKU accuracy is not measured here.

## Decision

Training infrastructure and artifact: PASS.
MK-7 held-out lens generalization: PASS on the measured MK-7 subset.
Full knowledge-task acceptance: PARTIAL until an explicit MKU-containing challenge set and broader semantic evaluator are run.
## MKU Targeted Repair

- Targeted slice: `Q:\Colibri\training\datasets\mk7\v0.2-mku-slice`
- New isolated adapter: `Q:\Colibri\training\runs\mk7\v0.2-mku-micro`
- Micro-overfit: 8/8
- Fresh reload: PASS
- First-formula accuracy: 8/8
- Raw exact accuracy: 0/8 because generation repeated after the first complete formula
- Parser decision: `MKU-PASS-AFTER-PARSER-FIX`

The general v0.1 adapter remains frozen; the MKU repair is isolated and must be selected explicitly for exact-formula requests.