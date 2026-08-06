# Training Troubleshooting

## Evidence-first rule

Never diagnose the whole machine from one Python executable, one terminal, or one failed command. Identify the exact environment, command, model, dataset, and output path first.

## Failure classes

| Class | Typical evidence | First diagnostic action |
|---|---|---|
| Environment mismatch | Package exists elsewhere but import fails here | Print Python executable and package paths |
| CUDA mismatch | `nvidia-smi` works but PyTorch cannot see CUDA | Inspect the active PyTorch build and runtime |
| Model identity mismatch | Folder name and `config.json` disagree | Read config and architecture fields |
| Data/schema failure | Parser, role, JSON, or tokenization errors | Validate a single record deterministically |
| Precision incompatibility | NaN loss, unsupported operation, device errors | Test conservative precision and CPU loading |
| Memory pressure | CUDA out-of-memory or system paging | Measure peak usage and reduce one variable |
| LoRA target mismatch | Target modules not found | Inspect actual module names before training |
| Circular evaluation | Great scores on generated examples only | Use held-out and independent evaluation |
| Output overwrite | Previous run disappears or becomes ambiguous | Use isolated immutable run directories |

## Diagnostic sequence

```text
1. Record the exact command.
2. Record the exact Python executable.
3. Record package versions.
4. Record model and dataset paths.
5. Reproduce on one record or a few steps.
6. Classify the failure layer.
7. Change one major variable.
8. Preserve the result and conclusion.
```

## Common mistakes to avoid

- Installing packages before discovering the working environment.
- Treating `nvcc` absence as proof that GPU training cannot work.
- Treating `nvidia-smi` success as proof that PyTorch CUDA works.
- Using GGUF as the default source for Transformers training.
- Mixing Gemma conversational formatting with FunctionGemma tool formats.
- Enabling FP16 on CPU.
- Introducing QLoRA before proving ordinary LoRA memory requirements.
- Declaring success from training loss without artifact reload and evaluation.
- Deleting failed runs before extracting the lesson.

## Root-cause record

Every resolved issue should add a compact entry to [LESSONS-LEARNED.md](./LESSONS-LEARNED.md):

```text
Symptom
Incorrect assumption
Confirmed root cause
Evidence
Fix
Prevention
Affected runs
```
