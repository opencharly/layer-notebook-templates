# AGENTS.md — layer-notebook-templates

Standalone candy repo for the `notebook-templates` data layer — the starter
`.ipynb` templates seeded into the workspace volume of a Jupyter image at deploy
time. It was the first data-only candy in the project. The candy lives in
`charly.yml` at the repo root: the `data:` mapping into the `workspace` volume,
the `plan:` `check:` assertions, and the embedded `skill:` entity projected into
the marketplace corpus as `/charly-jupyter:notebook-templates`.

Canonical files:

- `charly.yml` — the `notebook-templates:` candy entity and the
  `notebook-templates-skill:` skill entity.
- `data/notebooks/` — the starter notebooks.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-jupyter:notebook-templates` — the owning skill. The data-candy pattern
  and how starter content reaches a bind-backed volume. Load before editing or
  troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the `data:` field, and service
  declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence — they assert
  the workspace volume is mounted as a directory and at least one starter
  template is present. A change to the templates must keep those checks honest.

## Modify this repo

- Edit the `notebook-templates:` candy entity AND the
  `notebook-templates-skill:` skill entity in `charly.yml` together. The skill is
  the projected usage source, so a data, path, or behaviour change not mirrored
  in the skill leaves the corpus stale.
- This candy maps data to the volume **root** (no `dest:`); keep that consistent
  with the `plan:` checks and the skill.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo — read it
  before landing.
- Release history lives in `CHANGELOG/`.
