# notebook-templates

Starter notebook templates as a charly *data layer* — a small set of `.ipynb`
files seeded into the workspace volume at deploy time, so a fresh JupyterLab pod
has runnable examples on first open.

The `notebook-templates` candy ships no packages, no services and no
dependencies. It was the **first data-only candy** in the project: its `data:`
block copies `data/notebooks/` into the root of the `workspace` volume, and
consumers (the `jupyter` and `jupyter-ml` images) compose this layer to ship the
templates.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `notebook-templates` |
| Type | Data-only — no packages, no services, no dependencies |
| Volume | `workspace` → `/workspace` (supplied by the jupyter base) |
| Data | `data/notebooks` → `workspace` volume (root) |
| Templates | `getting-started.ipynb` |

## How it works

The candy maps a directory of files to a named volume with the `data:` field:

```yaml
data:
  - src: data/notebooks
    volume: workspace
```

At build time the contents are staged into `/data/workspace/` inside the box. At
deploy time, when the volume is bind-backed (`charly config --bind workspace`),
`charly config` copies the staged data from the box into the host-backed volume
directory — seeding the volume with starter content.

## How to use it

Compose the layer in a box's `candy:` list:

```yaml
jupyter:
  candy:
    base: fedora-nonfree
    candy:
      - '@github.com/opencharly/layer-notebook-templates:v2026.240.0201'
```

Then deploy with a bind-backed workspace volume:

```bash
charly config jupyter --bind workspace
charly start jupyter
```

## Layout

- `charly.yml` — the `notebook-templates:` candy entity (the `data:` mapping and
  the `plan:` checks) plus the embedded `skill:` entity.
- `data/notebooks/` — the starter notebooks.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-jupyter:notebook-templates` — the data-candy pattern and
  how starter content reaches a volume.
- Consuming boxes: `/charly-jupyter:jupyter`, `/charly-jupyter:jupyter-ml`,
  `/charly-jupyter:jupyter-ml-notebook`.
- Sibling data layers: `/charly-jupyter:notebook-ollama`,
  `/charly-jupyter:notebook-openrouter`,
  `/charly-jupyter:notebook-llm-on-supercomputers`.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.
