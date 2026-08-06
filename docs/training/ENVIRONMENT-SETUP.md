# Environment Setup and Discovery

## First rule

Do not assume that the first `python` found in `PATH` is the training environment.

A reliable audit must inspect:

- Windows Python installations;
- project-local `.venv`, `venv`, and `env` folders;
- Conda environments;
- WSL distributions;
- Docker images and containers;
- package managers such as `uv`, Poetry, and Pipenv;
- project scripts that activate or reference a specific environment.

## Evidence to collect per environment

| Category | Required evidence |
|---|---|
| Identity | Environment name, path, Python executable, Python version |
| Core ML | PyTorch version, CUDA runtime, cuDNN, GPU visibility |
| Training | Transformers, Datasets, Accelerate, PEFT, TRL |
| Optional | bitsandbytes, SentencePiece, logging integrations |
| Reproducibility | requirements, lock file, environment file, package snapshot |

## Decisive CUDA test

The presence of `nvidia-smi` proves that the NVIDIA driver can see the GPU. It does not prove that a particular Python environment can use CUDA.

The decisive environment-level check is:

```python
import torch
print(torch.__version__)
print(torch.cuda.is_available())
print(torch.version.cuda)
print(torch.cuda.get_device_name(0) if torch.cuda.is_available() else "CPU")
```

The CUDA Toolkit compiler `nvcc` is not always required for prebuilt PyTorch packages. It should not be installed merely because it is absent.

## Environment isolation

Once the active environment is discovered, preserve it before changing it:

```text
Environment path
Python version
Package list
PyTorch build
CUDA runtime
Activation command
Project scripts using it
Last confirmed run
```

Create a new environment only when the existing one is broken, irreproducible, or unsuitable. Do not overwrite a previously working environment during troubleshooting.

## Hardware baseline

Current known machine profile:

```text
Windows 11 Pro
Intel Core i7-8750H
47.76 GB RAM
NVIDIA Quadro P2000
4 GB VRAM
Compute capability 6.1
WSL2 available
```

This profile favors:

- short smoke tests;
- conservative sequence lengths;
- batch size 1 initially;
- LoRA as the first GPU baseline;
- careful compatibility checks for old GPU architecture;
- CPU and RAM offload only when measured and acceptable.
