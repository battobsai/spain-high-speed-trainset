# Copilot instructions — spain-high-speed-trainset

Purpose: give AI coding agents immediate, actionable context for working on this OpenTTD trainset.

- **Big picture:** This repository is an OpenTTD GRF trainset. The source is the NML file `spanish-high-speed-trains.nml` which defines GRF metadata, runtime parameters, templates and spritesets. Graphics live in `gfx/` (PNGs used by the NML) and editable sources are in `gimp/` (GIMP XCFs). Language strings are in `lang/`.

- **Build (Windows):** use the included batch helper: `compile.bat` (runs `nmlc -c --grf spanish-trains.grf spanish-high-speed-trains.nml`). If adding or renaming sprites, update the NML paths before compiling. See `export-test.bat` to copy generated GRF into an OpenTTD `newgrf` folder for manual testing.

- **Key files / dirs to inspect first:**
  - `spanish-high-speed-trains.nml` — primary source (GRF config, templates, spritesets, params)
  - `spanish-trains.grf` — generated artifact (output of compile.bat)
  - `gfx/` — PNG assets referenced by the NML (e.g. `rails_overlays.png`, `lc_right.png`, `depot_normal.png`)
  - `gimp/` — source XCF files for art (edit here, export to `gfx/` as needed)
  - `lang/` — `english.lng`, `spanish.lng` contain `STR_...` keys used by the NML
  - `compile.bat`, `export-test.bat` — build & deploy helpers

- **Common patterns you'll modify or extend:**
  - `spriteset(name, "gfx/…") { ... }` — NML spritesets; add new sprites by creating PNGs in `gfx/` and adding entries.
  - `template tmpl_*()` — reusable sprite templates; prefer adding small templates rather than duplicating sprite entries.
  - `param N { ... }` blocks and `switch(..., param_X)` — runtime configuration exposed to the player; keep language keys in `lang/` in sync when changing names/values.

- **Conventions & gotchas (project-specific):**
  - Keep `grfid`, `version`, and `min_compatible_version` inside `grf { ... }` consistent with OpenTTD compatibility rules; bump `version` when format changes.
  - Do not rename `STR_...` tokens in NML without updating `lang/*.lng`.
  - Graphics workflow: edit `.xcf` in `gimp/`, export as PNG into `gfx/`, then run `compile.bat`.
  - NML expects specific sprite geometry (x,y,w,h,offs...). Use existing `template tmpl_vehicle_*` entries as examples.

- **How to test changes locally:**
  1. Edit `gimp/*.xcf` and export required PNG(s) into `gfx/`.
  2. Run `compile.bat` to create `spanish-trains.grf`.
  3. Run `export-test.bat` (or copy the GRF into your OpenTTD `newgrf` folder) and launch OpenTTD to visually verify.

- **Search tips for code edits:**
  - Find where a sprite is used: search for `spriteset(` or the PNG filename under `gfx/`.
  - Find strings: search for `STR_` tokens to locate language keys in `lang/`.
  - Inspect `template` blocks to reuse geometry instead of duplicating entries.

- **Examples from repo:**
  - `spriteset(track_underlays, "gfx/rails_overlays.png") { ... }` — shows underlay sprites usage.
  - Parameters like `param_train_cost` are defined in `grf` and used in `basecost { PR_BUILD_VEHICLE_TRAIN: (param_train_cost); }`.

If anything important is missing or any workflow differs from your environment, tell me which parts to expand (build environment, automated tests, CI, etc.).
