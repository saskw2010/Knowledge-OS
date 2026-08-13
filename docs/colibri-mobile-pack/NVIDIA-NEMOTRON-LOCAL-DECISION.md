# NVIDIA Nemotron Local Decision — 2026-08-13

Status: `VERIFIED` for model availability; `NO-DOWNLOAD` decision locally.

## Exact candidates

### Nemotron-Mini-4B-Instruct

- Official Hugging Face repository: `nvidia/Nemotron-Mini-4B-Instruct`.
- Open weights under the NVIDIA Community Model License.
- 4B parameters, 4096-token context, English-focused, optimized for RAG,
  roleplay, and function calling.
- Official trainable checkpoint is about 8.38 GB per BF16 weight format.
- NVIDIA also advertises an on-device quantized runtime near 2 GB, but its
  downloadable NIM endpoint is not available from NVIDIA Build.

### NVIDIA-Nemotron-3-Nano-4B

- Official BF16 and FP8 weights exist under the NVIDIA Nemotron Open Model
  License.
- English and coding-focused hybrid Mamba/Transformer model.
- BF16 is the trainable form; FP8 is not a practical Pascal/P2000 training
  route. Official BF16 weights are too large for a 4 GB P2000 training run.

## Machine decision

| Use | Verdict | Reason |
|---|---|---|
| Local Q4 inference | POSSIBLE-BUT-UNTESTED | A community GGUF may fit, but exact runtime support and Arabic quality are unverified. |
| Local FP32/FP16 training | BLOCKED-BY-HARDWARE | Four-billion-parameter trainable weights exceed the 4 GB P2000 training envelope. |
| Local QLoRA | UNRESOLVED | P2000 software stack is not validated and the training-lab environment has no accepted run. |
| Online fine-tuning | CANDIDATE | Use BF16/trainable weights on a modern online GPU after the dataset and evaluator are proven. |
| MK7 replacement | NO-GO NOW | English focus and no frozen MK7 evidence; current constrained classifier is stronger evidence. |
| Coding assistant comparison | LATER | Phi-4 Mini already works locally; a Nemotron download is not justified before a larger executable benchmark. |

## Decision

Do not download Nemotron to `Q:` now. The drive has only about 8.34 GiB free,
and the available evidence does not show an advantage over the already-local
Phi-4 Mini. If Nemotron is evaluated next, prefer either NVIDIA's hosted API for
a no-download smoke or place a verified Q4 GGUF on `H:` after that drive becomes
available. Trainable Nemotron weights belong to the Online GPU candidates lane.

## Primary sources

- https://huggingface.co/nvidia/Nemotron-Mini-4B-Instruct
- https://build.nvidia.com/nvidia/nemotron-mini-4b-instruct/modelcard
- https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-4B-BF16
- https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-4B-FP8

