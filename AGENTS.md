# ComfyUI-Wheels Maintenance Guide

This file contains repository maintenance instructions for AI agents and other
automation. Keep end-user installation instructions in `README.md`; keep
maintenance workflow details here.

## Repository layout

- Keep every CUDA/PyTorch build combination on the `main` branch.
- Give each combination its own top-level index directory using this format:
  `cu<CUDA digits>torch<Torch major.minor>`.
- Example: CUDA 13.2 with PyTorch 2.13 uses `cu132torch2.13/`.
- Do not create a branch per CUDA or PyTorch version.
- Store each wheel beside its package `index.html` under
  `<build combination>/<normalized package name>/`.
- Publish GitHub Pages from the root of the `main` branch.

Example index layout:

```text
cu132torch2.13/
├── index.html
├── flash-attn/
│   ├── index.html
│   └── flash_attn-2.8.4-cp314-cp314-linux_x86_64.whl
└── sageattention/
    ├── index.html
    └── sageattention-2.2.0-cp314-cp314-linux_x86_64.whl
```

## Wheel naming and metadata

- Use the standard wheel filename format:
  `<distribution>-<version>-<python tag>-<abi tag>-<platform tag>.whl`.
- Do not add CUDA or PyTorch identifiers to the package version or filename.
  The top-level index directory identifies that build environment.
- The version in the filename must exactly match the `Version` field inside the
  wheel's `METADATA` file.
- Do not modify the contents of an upstream wheel merely to change its version.
- Preserve the original Python, ABI, and platform compatibility tags.
- Track every `.whl` file with Git LFS. Before committing, verify with:

```bash
git check-attr filter -- path/to/package.whl
git lfs ls-files
```

## Simple package index rules

- Each build directory is the root of a PEP 503-compatible static index.
- Its `index.html` links to each normalized package directory.
- Normalize package directory names according to Python packaging rules:
  replace runs of `.`, `_`, or `-` with one `-`, then lowercase the result.
- Therefore, the distribution `flash_attn` uses the index directory
  `flash-attn/`.
- Every package `index.html` must contain direct links to compatible wheel
  files for only that CUDA/PyTorch combination.
- Prefer absolute GitHub raw links in package index pages. Point them at the
  `main` branch and the exact wheel filename.
- When renaming, adding, or removing a wheel, update every affected index page
  in the same commit. Never leave an index link pointing at a missing file.

Example package link:

```html
<a href="https://github.com/cruise711808/ComfyUI-Wheels/raw/refs/heads/main/cu132torch2.13/flash-attn/flash_attn-2.8.4-cp314-cp314-linux_x86_64.whl">flash_attn-2.8.4-cp314-cp314-linux_x86_64.whl</a>
```

## Adding a build combination

1. Confirm the exact Python, PyTorch, CUDA, platform, and architecture versions
   used to build every wheel.
2. Inspect each wheel's internal `METADATA` and confirm its name and version.
3. Create the top-level build index, for example `cu128torch2.10/`.
4. Create one normalized package directory and `index.html` per package.
5. Copy each wheel beside its package `index.html` and confirm Git LFS tracking.
6. Add every package link to the build index root.
7. Validate the HTML links and wheel filenames locally.
8. Commit the wheels and index pages together, then push `main`.
9. Wait for GitHub Pages to finish deploying before testing the public URL.

## Validation

Inspect wheel metadata without extracting or modifying the wheel:

```bash
unzip -p path/to/package.whl '*/METADATA' | sed -n '1,20p'
```

Confirm that an index page is published:

```bash
curl -L --fail https://cruise711808.github.io/ComfyUI-Wheels/cu132torch2.13/flash-attn/
```

Resolve packages without changing an environment:

```bash
uv pip install \
  --dry-run \
  --python /path/to/venv/bin/python \
  --extra-index-url https://cruise711808.github.io/ComfyUI-Wheels/cu132torch2.13/ \
  --reinstall \
  --no-deps \
  'flash-attn==2.8.4' \
  'sageattention==2.2.0'
```

Use `--no-deps` for these binary extension wheels so validation or installation
does not replace the target environment's existing PyTorch installation.

## Publishing checks

- The repository is public because the current GitHub plan does not support
  Pages for this repository while private.
- Changing repository visibility or the Pages publishing source is an external,
  potentially disruptive action. Do not change either without explicit user
  authorization.
- After pushing an index update, verify the Pages deployment and public package
  pages before telling the user to install from the index.
- Do not commit build caches, temporary extraction directories,
  `build-info.json`, or generated diagnostic output.
