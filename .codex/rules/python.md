# Python rules

## Structure

Keep each assignment self-contained in its notebook. Use small, named notebook
functions for model loading, text encoding, denoising, decoding, training,
metrics, and visualization. Do not create duplicate scripts or source modules
unless the user asks for a package-based structure.

Model downloads, training, and inference must occur only in explicitly executed
cells. Import and configuration cells must not start expensive work.

## Code quality

- Use type hints for public functions.
- Add concise docstrings where behavior or tensor contracts are non-obvious.
- Name tensors by purpose, not by temporary implementation detail.
- Document expected tensor shapes at component boundaries.
- Prefer small functions with explicit inputs and outputs over global state.
- Do not hard-code local paths, credentials, model identifiers, devices, dtypes,
  prompts, seeds, resolutions, or step counts.
- Keep experiment values in one clearly labeled configuration cell.

## Dependencies and devices

- Pin or constrain dependencies in the selected dependency file.
- Record the Python, PyTorch, Diffusers, Transformers, Accelerate, PEFT, and CLIP
  package versions used for reported results.
- Select CPU, CUDA, or MPS explicitly and handle unsupported precision safely.
- Use inference mode for inference and avoid unnecessary gradient allocation.

## Reproducibility

Use a seeded generator where supported. Each reported run must record:

- Code revision and dependency versions.
- Base model identifier and revision.
- LoRA checkpoint and weight when applicable.
- Prompt, negative/null prompt, seed, image size, and batch size.
- Sampler/scheduler, steps, CFG scale, dtype, and device.
- Metric model/version and aggregation method.

Display canonical run settings in the notebook and write result metadata as JSON
or CSV beside ignored outputs. Do not rely on filenames alone.
