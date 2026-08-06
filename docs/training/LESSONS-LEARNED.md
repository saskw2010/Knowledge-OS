# Lessons Learned

This file preserves durable corrections and discoveries so the project does not repeat the same mistake.

## 2026-08-06 — Environment audit scope

**Symptom**  
An audit reported that PyTorch was missing.

**Incorrect assumption**  
The result was interpreted as proof that PyTorch was absent from the entire machine.

**Confirmed root cause**  
The audit tested only `C:\Python313\python.exe`. Other Python environments, WSL distributions, Conda environments, Docker images, and project-local virtual environments were not inspected.

**Evidence**  
The report named the exact Python executable and separately showed that WSL environments existed.

**Correction**  
State the result narrowly: PyTorch was not detected in that specific Python 3.13 environment.

**Prevention**  
All future environment audits must inventory every relevant runtime before making machine-wide claims.

---

## Standing lessons

### 1. Folder names are not model identity

Read `config.json`, tokenizer configuration, architecture fields, adapter configuration, and provenance. A directory called `functiongemma-270m-it-training` may be a full model, adapter, merged output, or incomplete run.

### 2. Loss is not success

A run is successful only after the artifact reloads and passes fixed evaluation.

### 3. QLoRA is conditional

QLoRA is useful under memory pressure or with larger models. It is not automatically the stage after LoRA.

### 4. Small models require narrow tasks

A 270M model should first prove one structured capability. Broad knowledge expansion and multiple unrelated behaviors can hide whether training worked.

### 5. Failed experiments are project assets

Preserve the command, environment, logs, root cause, corrective action, and prevention rule.
