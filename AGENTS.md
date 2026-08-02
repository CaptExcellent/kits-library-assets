# Asset repository agent guide

## Scope

These instructions apply to the independent `kits-library-assets` repository.
Also follow the workspace-level `AGENTS.md` when it is available.

## Repository role

This repository owns the original artwork served by the Kits Library:

```text
kits/<kit-name>/
  Asset Name.png
  Asset Name.svg
  Optional kit example.jpg
```

Catalog metadata, search rules, editor code, and generated manifests belong to
`kits-library-platform`, not this repository.

## Asset integrity

- Preserve original filenames unless a coordinated migration explicitly
  requires a rename.
- Prefer an SVG and PNG pair with the same basename for individual assets.
- Preserve intrinsic aspect ratios and transparent backgrounds.
- SVG files must parse as XML and must not reference missing local resources.
- Do not convert vectors to raster images as a shortcut.
- Do not add `.DS_Store`, temporary exports, editor caches, or unrelated files.
- Kit example JPG/PNG files are compositions, not individual catalog objects.
- Do not delete apparent duplicates without checking metadata references and
  the duplicate review queue in the platform repository.

## New or changed kit checklist

1. Confirm the kit folder name uses the existing `<name>-kit` convention.
2. Inventory PNG, SVG, and example files.
3. Confirm expected PNG/SVG basename pairs.
4. Validate SVG files:

   ```sh
   find kits/<kit-name> -name '*.svg' -print0 | xargs -0 xmllint --noout
   ```

5. Review unusually large files, embedded raster data, and broken transparency.
6. Commit and push this repository before updating metadata or the pinned
   gitlink in `kits-library-platform`.

If a source file is intentionally excluded from the catalog, document that in
the platform import/build logic rather than silently deleting it here.

## Git discipline

- This is a separate Git repository even when checked out inside the platform
  workspace.
- Inspect `git status -sb` here before staging.
- Stage the intended kit paths explicitly.
- Keep `.DS_Store` and unrelated assets out of the commit.
- After pushing, give the exact commit SHA to the platform change.
