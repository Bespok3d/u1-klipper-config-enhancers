# Object Processing

Turns on Moonraker's file-manager object processing, which extracts per-object information
from G-code so features like **adaptive bed mesh** and exclude-object can work.

## What it does

- Sets `[file_manager] enable_object_processing: True` in Moonraker.

## Using it with adaptive mesh

Enabling this alone is not enough. Your slicer start G-code must also call:

```
BED_MESH_CALIBRATE ADAPTIVE=1
```

so Klipper only meshes the area your objects cover. The **Force Bed Mesh (Adaptive)**
plugin sets this up for you.

## Heads up

- Processing large G-code files (over ~100 MB) can be slow and memory-hungry, which delays
  uploads and the start of a print.
- If your slicer supports **Exclude Object** directly, prefer that; it is more reliable and
  needs no server-side processing.

## Notes

Restarts Moonraker on install.
