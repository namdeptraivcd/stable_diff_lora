# Stable Diffusion LoRA assignments

The project is organized around two self-contained notebooks:

```text
stable_diff_lora/
├── notebooks/
│   ├── assignment2_4.ipynb   # manual DDIM + CFG comparison
│   └── assignment2_5.ipynb   # LoRA + CLIP-I/CLIP-T + ComfyUI handoff
├── data/
│   ├── README.md
│   └── Itay/                 # local personal dataset; ignored by Git
├── workflows/
│   ├── baseline.json
│   ├── optimized.json
│   └── README.md
├── requirements.txt
├── AGENTS.md
└── LICENSE
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

Open the notebooks from `notebooks/` and run each one from top to bottom.

## Assignment 2.4

`notebooks/assignment2_4.ipynb` contains the entire submission:

- Separate tokenizer, CLIP text encoder, U-Net, DDIM scheduler, and VAE loading.
- Conditional and unconditional prompt embeddings.
- Two explicit U-Net forward passes per denoising step.
- CFG scales `1, 3, 5, 7.5, 12, 20` using the same initial latent.
- Comparison grid and sweet-spot analysis.

No high-level Diffusers pipeline is used.

## Assignment 2.5

Read `data/README.md` before opening `notebooks/assignment2_5.ipynb`. The notebook
contains the complete 5–10 image LoRA workflow, matched base/LoRA generation,
CLIP-I and CLIP-T evaluation, and the ComfyUI handoff.

Set `RUN_TRAINING = True` only after completing a one-step smoke test. Personal
images, outputs, and model weights must remain outside Git.

## ComfyUI

ComfyUI workflows remain as JSON because ComfyUI must import them directly:

- `workflows/baseline.json` — 50-step DDIM baseline.
- `workflows/optimized.json` — DPM++ 2M Karras, latent upscale, and refinement.

See `workflows/README.md` for model placement and benchmarking instructions.
