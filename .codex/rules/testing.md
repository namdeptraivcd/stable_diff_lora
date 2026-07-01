# Testing rules

## General validation

Run the smallest relevant checks for every change.

- Parse notebooks as JSON and compile every code cell before delivery.
- Review Markdown links, notebook ordering, and command examples.
- Parse every saved configuration and ComfyUI workflow.
- Never report a model-dependent cell as passing unless it was actually run.

## Required tests

Tests for manual inference must cover:

- Conditional and unconditional text-embedding shapes.
- The CFG formula with a small known tensor.
- Two distinct U-Net calls per denoising step.
- Reuse of an identical initial latent across guidance scales.
- DDIM scheduler-step integration.
- VAE output shape and valid image range.
- Fixed-seed determinism where the backend supports it.

Tests for training and evaluation must cover:

- A one-step LoRA train, save, and reload smoke test.
- Confirmation that intended LoRA parameters train while base parameters remain
  frozen.
- Normalization and correct image/text pairing in CLIP metrics.
- Per-sample and aggregate metric output schemas.

## Runtime smoke tests

Before an expensive run, use reduced resolution and steps to exercise manual
inference end to end. Exercise LoRA loading and one complete ComfyUI generation
before evaluating the full workflow.

## Definition of done

A deliverable is complete only when its command and configuration are documented,
outputs are traceable to their settings, relevant checks pass, and skipped checks
or limitations are stated plainly.
