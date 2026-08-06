# Build in Public Video Brief — Gemma 3 270M IT LoRA v0.1

> **Purpose:** A direct analytical video that turns the experiment into a credible technical and business story.  
> **Recommended format:** 8–12 minutes, screen recording plus live explanation.  
> **Audience:** clients, technical partners, developers, potential contributors, and funding conversations.

## Working titles

1. **We trained Gemma 3 270M locally on a 4 GB GPU — then our evaluator said 0%**
2. **How a parser bug hid a successful local LoRA experiment**
3. **Building Sky365 in public: from 0% failure to 100% after debugging the measurement**
4. **Can a 270M model learn ERP intents on old hardware? Our controlled test**

## Thumbnail / opening line

```text
The model looked like a total failure.
The training was not the problem.
Our measurement was.
```

## Core business message

Sky365 is building a reproducible local AI training capability rather than depending only on large hosted models. The first controlled experiment trained a 270M instruction model on modest hardware, exposed an evaluator defect, corrected it transparently, and preserved the full engineering evidence.

Do not lead with “100% accuracy” alone. Lead with the process that made the number trustworthy.

## Video structure

### 0:00–0:35 — Hook

Suggested speech:

> We trained a small Gemma model locally on a Quadro P2000 with only 4 GB VRAM. The adapter saved, reloaded, and the loss dropped almost to zero. Then the evaluator told us the model scored zero percent on validation and test. Instead of retraining blindly, we audited the raw output—and discovered that the model was right while the parser was wrong.

Show:

- `PASS-AFTER-PARSER-FIX` result;
- GPU name and 4 GB VRAM;
- original `0%` result crossed out;
- final `20/20` test result.

### 0:35–1:30 — Why we did this

Explain:

- Sky365 needs small local specialist models for focused enterprise tasks;
- the aim was not a general chatbot;
- the first task was ERP intent classification;
- success required a reproducible pipeline, not only a low training loss.

On screen:

```text
Arabic / English / Mixed request
            ↓
{"intent":"approved.intent"}
```

### 1:30–2:25 — The fixed experiment contract

Show the choices:

```text
Model: google/gemma-3-270m-it
Method: LoRA
Dataset: 64 train / 16 validation / 20 test
Classes: 4
QLoRA: no
FunctionGemma: no
GPU: Quadro P2000 4 GB
```

Explain why separating Gemma from FunctionGemma mattered. Mention that identity confusion can invalidate a whole training record.

### 2:25–3:20 — The gates before training

Display the methodology:

```text
Identity
→ Environment
→ Dataset validation
→ One-step preflight
→ Micro-overfit
→ Controlled run
→ Fresh-process evaluation
```

Emphasize:

- one exact Python environment;
- no dependency on PATH guesses;
- adapter save and reload in a new process;
- held-out test set frozen before training.

### 3:20–4:15 — Micro-overfit proof

Show:

```text
8 examples
50 steps
Loss: 5.782916 → 0.000636
Strict: 8/8
Parsed: 8/8
Valid JSON: 8/8
```

Suggested explanation:

> The micro-overfit was not a quality benchmark. It was a diagnostic gate. If the model could not learn eight examples, increasing the dataset would only hide a pipeline problem.

### 4:15–5:20 — Controlled LoRA run

Show:

```text
LoRA r=8
alpha=16
dropout=0.05
4 epochs
Train loss: 0.299114 → 0.000063
Peak VRAM: ~2.1 GB
Adapter reload: PASS
```

Then reveal:

```text
Validation accuracy: 0%
Test accuracy: 0%
```

Pause briefly. Explain why this contradicted the earlier evidence.

### 5:20–6:45 — The investigation

Show the raw repeated generation:

```text
{"intent":"employee.create"}<end_of_turn>
{"intent":"employee.create"}<end_of_turn>
...
```

Explain the old parser behavior:

```text
first {  →  last }
```

It combined several valid JSON objects into one invalid value.

Show the fix:

```text
Stop at first <end_of_turn>
OR
Extract first balanced JSON object
```

State clearly:

- no retraining;
- no dataset modification;
- no constrained decoding;
- no weight changes;
- only the evaluator was corrected.

### 6:45–7:35 — Verified result

Show a clean result board:

```text
Train audit: 12/12
Validation: 16/16
Held-out test: 20/20
Valid JSON: 48/48
Wrong intents: 0
Parser failures: 0
Decision: PASS-AFTER-PARSER-FIX
```

Say:

> The earlier zero was a false negative. The important lesson is not that we found a way to make the number look better. The lesson is that we preserved the raw predictions and could prove exactly why the evaluator was wrong.

### 7:35–8:35 — What this means for Sky365

Connect to business:

- local module-level classifiers;
- lightweight routing before expensive models;
- Arabic and bilingual enterprise workflows;
- lower inference cost for narrow tasks;
- auditable model behavior;
- reusable training and evaluation lab.

Avoid claiming production readiness.

### 8:35–9:20 — What remains unproven

Say directly:

> This was a controlled synthetic dataset. We have not yet proven wide real-world robustness, multi-intent behavior, adversarial prompts, or hundreds of ERP intents.

Show:

```text
Controlled experiment: PASS
Production readiness: NOT YET
```

### 9:20–10:00 — Next Build in Public episode

Announce the unseen challenge set:

```text
60 entirely new prompts
Dialect
Spelling noise
Ambiguity
Out-of-domain
Multi-intent
Boundary cases
```

Thresholds:

```text
Valid JSON: 60/60
Overall: ≥ 85%
Known intents: ≥ 90%
Unknown: ≥ 80%
Parser failures: 0
```

End with:

> We are not building in public to show perfect demos. We are building in public to show decisions, evidence, failure analysis, and measurable progress. The next video will show whether this adapter survives prompts it has never seen.

## Screen assets checklist

- [ ] Training Lab landing page.
- [ ] Model identity and local path.
- [ ] Hardware/environment table.
- [ ] Dataset split and four intents.
- [ ] Preflight metrics.
- [ ] Micro-overfit loss and 8/8 result.
- [ ] Controlled-run loss and apparent 0% result.
- [ ] Raw repeated JSON output.
- [ ] Parser code before and after.
- [ ] Rescored 12/12, 16/16, 20/20 results.
- [ ] Next challenge-set plan.
- [ ] Repository links and experiment methodology document.

## Evidence rules for the video

- Never show API tokens, Hugging Face credentials, private client data, or machine secrets.
- Do not publish proprietary production datasets.
- Use the exact experiment numbers; do not round them into stronger claims.
- Say “controlled held-out test” rather than “the model is 100% accurate.”
- State that the dataset is small and synthetic.
- Preserve the original false result as part of the story.
- Clearly separate model learning, parser correctness, and production robustness.

## Reusable short-form clips

### Clip 1 — The 0% trap

> Our model scored zero percent, but every raw output started with the correct JSON. The parser was joining repeated objects into invalid JSON. One measurement fix changed the score—without retraining the model.

### Clip 2 — Why micro-overfit matters

> Before training one hundred examples, we forced the model to memorize eight. If it could not reach eight out of eight, the problem would be the training pipeline—not the dataset size.

### Clip 3 — Local AI on modest hardware

> We trained a 270M instruction model with LoRA on a 4 GB Quadro P2000. The point is not that this GPU is fast. The point is that disciplined scope can turn modest hardware into a useful R&D lab.

### Clip 4 — Build in public honestly

> Build in public is not publishing a perfect score. It is publishing the incorrect result, the raw evidence, the root cause, the fix, and the next harder test.

## Call to action options

### Client-oriented

> If your ERP has repetitive routing, classification, or structured extraction tasks, small private models may handle part of that workload locally before a larger model is involved.

### Developer-oriented

> The methodology and experiment record are public in the Knowledge-OS repository. Review the process, challenge the assumptions, and follow the unseen test.

### Partner/funding-oriented

> We are building an evidence-driven local AI stack for enterprise workflows. This is one narrow milestone in a larger system for datasets, training, evaluation, and governed deployment.

## Future addendum

After the challenge-set run, append:

- challenge-set composition;
- frozen-adapter hash;
- challenge metrics;
- top error categories;
- business interpretation;
- decision: release, iterate data, iterate config, or narrow scope.
