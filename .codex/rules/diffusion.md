# Stable Diffusion rules

## Assignment deliverables

The project must produce:

- Manual DDIM inference using individually connected Hugging Face Diffusers and
  Transformers components.
- A controlled CFG comparison at scales `1, 3, 5, 7.5, 12, 20`.
- An evidence-based CFG sweet-spot and over-guidance analysis.
- A LoRA trained on 5–10 self-collected, non-public-figure subject images.
- CLIP-I subject-fidelity and CLIP-T prompt-fidelity results.
- An importable, documented ComfyUI workflow using the trained LoRA.

## Manual inference

Assignment 2.4 must connect the tokenizer, CLIP text encoder, U-Net, DDIM
scheduler, and VAE manually. Loading individual pretrained components is
allowed. `StableDiffusionPipeline` and every other high-level inference pipeline
are forbidden for this implementation.

The implementation must:

1. Tokenize the text prompt with truncation and max-length padding, then encode
   the conditional CLIP embedding.
2. Tokenize an empty prompt identically, then encode the unconditional embedding.
3. Generate the initial latent once from a seeded generator and apply the
   scheduler's required initial scaling.
4. Configure `DDIMScheduler` and iterate over all timesteps.
5. Scale the latent model input when the scheduler requires it.
6. Run two distinct U-Net forward passes per timestep: conditional and
   unconditional. Do not replace them with one batched pass.
7. Apply CFG exactly as
   `uncond_noise + scale * (cond_noise - uncond_noise)`.
8. Pass the guided prediction and latent into the DDIM scheduler step, carrying
   its previous-sample output into the next iteration.
9. Apply the chosen model's VAE latent scaling convention, decode, map to image
   range, clamp, and save.

Keep tokenizer length, tensor shapes, latent dimensions, scheduler behavior, and
VAE scaling compatible with the exact base-model version.

## CFG experiment

Compare scales in the exact order `1, 3, 5, 7.5, 12, 20`. Hold prompt,
null/negative prompt, base model, scheduler, steps, resolution, seed, and initial
latent constant. Clone the same starting latent for every scale.

Save each image, a labeled comparison grid, and machine-readable settings. The
report must identify the preferred scale/range and the first tested scale where
quality degrades. Cite visible evidence such as oversaturation, clipped
highlights, excessive contrast, halos, geometry errors, texture loss, or reduced
naturalness. Distinguish prompt adherence from perceptual quality; do not assume
that `7.5` is automatically best.

## LoRA training

Use 5–10 self-collected images. Keep the base model frozen and train only the
documented LoRA target modules. Prepare consistent captions with a private
subject token and suitable class word, remove duplicates, and document the
crop/resize policy.

Record base model/revision, resolution, precision, seed, rank, alpha, dropout,
target modules, optimizer, learning rate and scheduler, batch size, gradient
accumulation, epochs/steps, augmentations, and checkpoint cadence. Run a one-step
train/save/reload smoke test first.

Monitor fixed prompts and seeds, retain intermediate checkpoints, and select the
final checkpoint using identity fidelity, diversity, and prompt editability—not
training loss alone.

## CLIP evaluation

- CLIP-I is cosine similarity between normalized generated-image embeddings and
  normalized held-out/reference subject-image embeddings.
- CLIP-T is cosine similarity between normalized generated-image embeddings and
  normalized corresponding-prompt text embeddings.

Use one documented CLIP model and preprocessing version throughout a comparison.
Evaluate multiple prompts and fixed seeds, including novel contexts, poses, or
styles. Compare the base model and selected LoRA under matched settings. Save
per-image scores, mean, standard deviation, generation settings, a labeled grid,
and qualitative notes. Treat CLIP scores as proxies rather than complete measures
of image quality.

## ComfyUI

Save an importable workflow JSON under `workflows/`. The graph must include
checkpoint loading, positive/negative prompt encoding, LoRA loading, latent
creation, sampling, VAE decoding, and image output. Expose LoRA weight, seed,
sampler/scheduler, steps, CFG, and resolution.

Prefer built-in nodes. Pin and document any custom nodes and tested ComfyUI
version/commit. Quality or speed additions may include an efficient sampler,
latent/tiled upscaling, a low-denoise refinement pass, runtime attention
optimizations, or caching, but each addition must be measured or visibly
justified. Compare the optimized graph with a simple baseline using matched
prompt and seed, recording hardware, runtime, peak memory when available, and
visual observations.
