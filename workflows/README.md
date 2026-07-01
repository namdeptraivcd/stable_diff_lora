# ComfyUI workflows

Two workflows are provided, using built-in nodes only:

- `baseline.json`: notebook-parity settings—DDIM, 50 steps, CFG 7.5, seed 42,
  LoRA strength 1.0, and 512×512.
- `optimized.json`: DPM++ 2M Karras at 20 base steps, latent upscale to
  768×768, then an 8-step low-denoise refinement pass.

The optimized graph spends fewer steps on the initial image and uses a short
second pass to recover detail after latent upscaling. Whether this is faster or
better depends on hardware and the subject; measure it rather than assuming.

## Setup

1. Install a current ComfyUI release.
2. Put the matching Stable Diffusion v1.5 checkpoint in
   `ComfyUI/models/checkpoints/`.
3. Copy `outputs/assignment2_5/lora/personal_subject_lora.safetensors` to
   `ComfyUI/models/loras/`. This is the ComfyUI-formatted export. Do not rename
   `pytorch_lora_weights.safetensors`: it uses Diffusers keys that ComfyUI may
   report as `lora key not loaded`.
4. Import either workflow JSON and select the installed checkpoint/LoRA in the
   loader nodes.
5. Keep prompt, seed, LoRA strength, and output size matched when comparing
   workflows.

The trained LoRA modifies U-Net attention only, so `strength_clip` stays at
`0.0`. After the first run, check the ComfyUI log and confirm that it contains
no `lora key not loaded` warnings.

## Benchmark table

Record at least three runs after one warm-up run:

| Workflow | Hardware | Resolution | Median time | Peak memory | Quality notes |
|---|---|---:|---:|---:|---|
| Baseline | | 512×512 | | | |
| Optimized | | 768×768 | | | |

Because the output resolutions differ, report that distinction clearly. For a
strict speed-only comparison, change the optimized upscale target to 512×512.
