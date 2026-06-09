# u1-klipper-config-enhancers

A small co-repo of config-only Bespok3d plugins for the Snapmaker U1: each one drops a single
Klipper or Moonraker config include and asks the daemon to restart the affected service. No
scripts, no patches, no binaries. They are the simplest worked example of the plugin model and a
good template for a multi-plugin repo that publishes its own sub-list.

Plugins:

- **cpu-temp** - adds the Rockchip SoC as a Klipper `temperature_sensor`.
- **object-processing** - enables Moonraker `enable_object_processing` for adaptive mesh.
- **purge-line-back** - moves the print-start purge line to the back of the bed.

## Layout

```text
u1-klipper-config-enhancers/
  cpu-temp/                 # one plugin = one dir; its name is the manifest .name
    manifest.json           # intent declarations + catalog metadata (single source of truth)
    files/                  # payload tree the daemon places on the printer
    doc/README.md           # rendered in-app; not deployed to the printer
  object-processing/
  purge-line-back/
  scripts/
    pack.sh                 # pack every <plugin>/ into dist/<name>-<ver>.b3
    generate-atom.mjs       # emit one plugin's index atom (--plugin <id>)
    assemble-list.mjs       # assemble the atoms into this repo's index.json sub-list
  .github/workflows/release.yml
  index.json                # the published sub-list (committed; referenced by main-index)
  dist/                     # build output (gitignored)
```

Each plugin declares WHAT, never HOW: a destination `class` (`klipper-config` / `moonraker-config`)
and a `restart` hook (`klipper` / `moonraker`). The adapter on the printer maps the class to a real
path and realizes the restart. See `Bespok3d/doc/anatomy-of-a-plugin.md` and `package-format.md`.

## Build locally

Prerequisites: `zip`, `jq`, and `shasum` (macOS) or `sha256sum` (Linux), plus `node` for the atom +
sub-list steps.

```sh
sh scripts/pack.sh                              # -> dist/<name>-<ver>.b3 for every plugin
node scripts/generate-atom.mjs --plugin cpu-temp   # -> dist/cpu-temp.atom.json (dry-run url)
node scripts/assemble-list.mjs                  # -> index.json from dist/*.atom.json
```

The monorepo's bundled dev index discovers these plugins directly from this tree (no publish step
needed for local development); see `Bespok3d/scripts/generate-index.mjs`.

## Releasing

Bump a plugin's `manifest.json` `version` and push to `main`. CI packs every `.b3`, cuts a GitHub
release per plugin, regenerates this repo's `index.json` sub-list, and registers the sub-list in
`Bespok3d/main-index` (a `lists/<repo>.json` `{name,url}` reference) so the official catalog picks
the plugins up. main-index is a list-of-lists; it references this sub-list by URL rather than
copying each atom.

Secret required: `MAIN_INDEX_TOKEN` (a fine-grained PAT with `contents:write` on
`Bespok3d/main-index`). GPG signing of the sub-list is deferred during private testing.
## Maintainership

These plugins are published and maintained by the Bespok3d org, and several of them repackage or
build on upstream source material. If you own the source material a plugin is based on and would
rather manage it yourself, you are welcome to contact the org to claim it back. The one condition is
that it stays actively maintained: a claimed plugin left to rot will be reclaimed so users are never
stranded on an abandoned package.
