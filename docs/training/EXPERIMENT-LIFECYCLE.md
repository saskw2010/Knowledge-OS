# Experiment Lifecycle

## Principle

Every training run is an experiment with an identity, inputs, configuration, outputs, metrics, and a conclusion. A folder without those elements is not reliable evidence.

## Recommended run structure

```text
runs/
└── YYYYMMDD-HHMM-<model>-<task>-<method>/
    ├── run-manifest.json
    ├── config/
    ├── logs/
    ├── checkpoints/
    ├── evaluation/
    ├── samples/
    └── conclusion.md
```

## Run manifest

Record at minimum:

```text
Run ID
Date and time
Git commit
Python environment
Model name and local path
Dataset name, version, and path
Training method
Device
Precision
Sequence length
Batch size
Gradient accumulation
Epochs or max steps
Learning rate
Seed
LoRA configuration
Output directory
Resume checkpoint
```

## Run stages

1. Preflight validation.
2. Baseline inference capture.
3. Training execution.
4. Artifact save.
5. Reload test.
6. Fixed evaluation.
7. Resource and timing summary.
8. Human conclusion.

## Failure handling

A failed run remains valuable if it records:

- exact error;
- environment and command;
- step at failure;
- last valid checkpoint;
- suspected root cause;
- evidence that supports or weakens the hypothesis;
- next smallest diagnostic action.

Do not overwrite failed outputs. Preserve them or mark them as incomplete.

## Comparison rule

Change one major dimension at a time whenever possible:

- dataset version;
- model;
- training method;
- learning rate;
- sequence length;
- LoRA rank;
- evaluation version.

Without controlled comparison, an apparent improvement cannot be attributed reliably.
