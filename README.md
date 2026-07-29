# u1-klipper-config-enhancers

A small co-repo of config-only Bespok3d plugins for the Snapmaker U1: each one drops a single
Klipper or Moonraker config include and asks the daemon to restart the affected service. No
scripts, no patches, no binaries. They are the simplest worked example of the plugin model and a
good template for a multi-plugin repo that publishes its own sub-list.

Plugins:

- **cpu-temp** - adds the Rockchip SoC as a Klipper `temperature_sensor`.
- **idle-timeout** - sets how long the printer stays awake before powering its motors down (overrides the stock idle_timeout).
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
  .github/workflows/release.yml
  index.json                # the published sub-list (committed; referenced by main-index)
  dist/                     # build output (gitignored)
```

Each plugin declares WHAT, never HOW: a destination `class` (`klipper-config` / `moonraker-config`)
and a `restart` hook (`klipper` / `moonraker`). The adapter on the printer maps the class to a real
path and realizes the restart.

## Build locally

Needs Node.js 20+. Builds run through the shared `Bespok3d/b3-builder` tool:

```sh
npm install github:Bespok3d/b3-builder
npx b3-builder build --source ./cpu-temp --atom-repo Bespok3d/u1-klipper-config-enhancers
# -> dist/cpu-temp-<ver>.b3 + dist/cpu-temp.atom.json
```

Drop `--source` to build every plugin in the repo at once.

## Releasing

Bump a plugin's `manifest.json` `version` and push to `main`. CI runs the `Bespok3d/b3-builder`
Action over the whole repo, which packs each `.b3`, cuts a release per plugin, assembles this repo's
`index.json` sub-list as `U1 Klipper Config Enhancers`, and registers it in `Bespok3d/main-index`
(`lists/<repo>.json`). Secrets: `MAIN_INDEX_TOKEN` (contents:write on main-index) and
`REGISTRY_SIGNING_KEY` (the org registry key the `b3-builder` Action signs each `.b3` and atom with).

## Maintainership

These plugins are published and maintained by the Bespok3d org, and several of them repackage or
build on upstream source material. If you own the source material a plugin is based on and would
rather manage it yourself, you are welcome to contact the org to claim it back. The one condition is
that it stays actively maintained: a claimed plugin left to rot will be reclaimed so users are never
stranded on an abandoned package.

## Licence

Copyright (C) 2026 unlucio and the Bespok3d contributors

This repo ships code from other projects offered under version 3 of the GNU General Public License,
with no option to use a later version, so version 3 of that licence covers every file in this repo.

This program is free software: you can redistribute it and/or modify it under the terms of version 3
of the GNU General Public License as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without
even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not,
see <https://www.gnu.org/licenses/>. The full text is in [LICENSE](LICENSE).

Bespok3d's own code elsewhere is AGPL-3.0-or-later. One licence covering this whole repo is a clarity
choice, so that nobody has to work out which file carries which terms. Version 3 of the GPL and
version 3 of the AGPL may be combined in a single work, and section 13 of each licence says so; what
cannot happen is code offered under version 3 of the GPL alone being re-offered under the AGPL.

Bespok3d is a project of the Bespok3d Organisation, which is not a legal entity. Copyright is held by
the individual authors named above.
