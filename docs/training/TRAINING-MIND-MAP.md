# Training Mind Map

This page is the conceptual map for Sky365 Tiny training. It separates proof, training, optimization, evaluation, and release so that complexity is introduced only when justified.

```mermaid
mindmap
  root((Sky365 Tiny))
    Define the task
      User intent
      Input format
      Expected output
      Failure boundaries
      Success metric
    Establish evidence
      Exact model identity
      Exact environment
      Exact dataset version
      Previous run inventory
      Hardware capability
    Validate data
      Legal eligibility
      Provenance
      Schema
      Deduplication
      Leakage checks
      Train validation test
    Prove the pipeline
      Model loads
      Tokenizer works
      Chat template works
      CPU smoke test
      Loss is finite
      Checkpoint saves
      Inference works
    Train the baseline
      GPU LoRA
      Fixed seed
      Isolated output
      Logging
      Evaluation
    Consider alternatives
      Full fine tuning
        Measure memory first
      QLoRA
        Only for memory pressure
      Quantization
        Primarily deployment
      Multi model router
        Separate specialists
      MoE
        Later research only
    Evaluate
      Task accuracy
      JSON validity
      Tool selection
      Arabic
      English
      Unknown handling
      Regression
    Release
      Adapter
      Merged model
      GGUF if required
      Model card
      Dataset card
      Run record
      Deployment test
    Learn
      Failure registry
      Root cause
      Corrective action
      Reusable lesson
```

## Decision gates

```mermaid
flowchart TD
    A[Do we know the exact active environment?] -->|No| B[Run discovery audit]
    A -->|Yes| C[Can model, tokenizer, and one sample load?]
    C -->|No| D[Repair loading path]
    C -->|Yes| E[Does a 2-20 step smoke test complete?]
    E -->|No| F[Diagnose data, device, precision, or script]
    E -->|Yes| G[Run LoRA baseline]
    G --> H{Pass fixed evaluation?}
    H -->|No| I[Classify root cause]
    I --> J{Data problem?}
    J -->|Yes| K[Repair dataset]
    J -->|No| L[Repair training or reconsider model capacity]
    K --> G
    L --> G
    H -->|Yes| M[Release baseline]
    M --> N{Measured need for optimization?}
    N -->|No| O[Stop and preserve success]
    N -->|Yes| P[Evaluate Full FT, QLoRA, quantization, or routing]
```

## Core truth

- CPU is a diagnostic baseline, not the production training target.
- LoRA is the default first GPU method.
- QLoRA is conditional, not a compulsory next stage.
- Full fine-tuning is measured rather than dismissed because 270M is small.
- MoE is outside the immediate success path.
- No run is successful without independent evaluation and a loadable artifact.
