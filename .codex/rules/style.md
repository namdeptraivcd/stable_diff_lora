# Project style rules

## Repository layout

Use this minimal layout unless the user explicitly requests a split package:

```text
notebooks/     one self-contained notebook per assignment
data/          local dataset documentation and ignored personal images
workflows/     importable ComfyUI JSON and setup notes
```

Keep Assignment 2.4 and Assignment 2.5 in their respective notebooks. ComfyUI
workflow JSON stays separate because it must be imported by ComfyUI. Do not
duplicate notebook logic into scripts, packages, configs, reports, or a web app
unless requested.

## Change style

- Keep changes focused and preserve unrelated user work.
- Prefer small, reviewable changes and explain non-obvious decisions.
- Follow existing naming and formatting conventions when they exist.
- Do not silently weaken assignment requirements for hardware limitations;
  document the limitation and provide a compliant reduced-size smoke test.
- Document setup and runnable commands in `README.md` as they are introduced.

## Artifacts and privacy

- Never commit secrets, credentials, model weights, raw datasets, personal
  images, face embeddings, caches, or bulk generated outputs.
- Add dataset, checkpoint, cache, output, and secret patterns to `.gitignore`
  before creating those artifacts.
- Use Git LFS only after the project explicitly adopts it.
- Keep reports lightweight and reference ignored artifacts by relative path.

Training images must be self-collected, must not depict celebrities or public
figures, and must have clear usage rights and consent. Remove unnecessary EXIF
metadata from derived training copies. Use a private subject token that does not
reveal the person's name. Never upload personal images, embeddings, or weights
without explicit user authorization.
