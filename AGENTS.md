# AGENTS.md

## Mission

Build a reproducible Stable Diffusion project for:

1. Manual classifier-free-guidance inference with Hugging Face Diffusers.
2. LoRA fine-tuning on 5–10 self-collected subject images.
3. CLIP-I and CLIP-T evaluation.
4. A ComfyUI inference workflow for the trained LoRA.

The assignment constraints are requirements, not suggestions. In particular,
do not use a high-level Diffusers pipeline for Assignment 2.4.

## Rule map

Before changing the project, read this file and the rules relevant to the task:

- [`.codex/rules/python.md`](.codex/rules/python.md) — Python structure,
  configuration, dependencies, and reproducibility.
- [`.codex/rules/testing.md`](.codex/rules/testing.md) — unit tests, smoke tests,
  validation, and completion criteria.
- [`.codex/rules/style.md`](.codex/rules/style.md) — repository organization,
  documentation, privacy, and collaboration style.
- [`.codex/rules/diffusion.md`](.codex/rules/diffusion.md) — manual Diffusers
  inference, CFG experiments, LoRA, CLIP evaluation, and ComfyUI.

If rules conflict, follow the assignment requirements in this file first. Do not
silently weaken a requirement to accommodate hardware or library limitations;
document the limitation and provide a compliant reduced-size smoke test.

## Core rules

- Keep changes focused and preserve unrelated user work.
- Never commit secrets, model weights, datasets, personal images, or generated
  outputs unless the user explicitly requests an appropriate storage strategy.
- Do not invent commands or claim checks were run when they were not.
- Document setup and runnable commands in `README.md` as they are introduced.
- Prefer configuration files and CLI arguments over hard-coded machine values.

## Current structure

- `notebooks/assignment2_4.ipynb` is the complete Assignment 2.4 deliverable.
- `notebooks/assignment2_5.ipynb` is the complete Assignment 2.5 deliverable.
- `data/Itay/` is private local data and must remain outside Git.
- `workflows/` contains the importable ComfyUI artifacts.

Do not recreate parallel scripts, source modules, configs, tests, or report files
unless the user explicitly asks to split the notebooks again. Keep each
assignment understandable and runnable from its single notebook.
