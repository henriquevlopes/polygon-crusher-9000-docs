# Polygon Crusher 9000

**Fast, clean mesh decimation for Blender — without destroying your normals.**

Polygon Crusher 9000 is a Blender addon that reduces mesh polygon count using the Fast Quadric Mesh Simplification (FQMS) algorithm. Unlike Blender's built-in Decimate modifier, it transfers the original surface normals back onto the result — so decimated meshes keep their smooth shading even at aggressive reduction levels.

---

## Key Features

- **FQMS engine** — industry-standard quadric error simplification, compiled as a native Windows executable for maximum speed
- **Normal preservation** — transfers original split normals onto the decimated mesh via Blender's Data Transfer modifier (`POLYINTERP_LNORPROJ`), keeping smooth shading intact
- **UV transfer** — optionally re-projects the original UV map onto the crushed result
- **Lossless mode** — removes only redundant geometry with zero visual change; ideal for cleaning up over-subdivided or imported hard-surface models
- **Live progress bar** — non-blocking modal operation so Blender stays responsive during long crushes; press **ESC** to abort cleanly
- **Session stats** — per-object triangle counts and timing shown after each crush

---

## Requirements

| | |
|---|---|
| **Blender** | 4.0 or later (tested on 5.1) |
| **OS** | Windows (the crusher engine is a native `.exe`) |

---

## Quick Links

- [Installation →](getting-started/installation.md)
- [Quick Start →](getting-started/quick-start.md)
- [Settings Reference →](settings.md)

---

## Support

Questions or issues? Email [henriquevlopes@gmail.com](mailto:henriquevlopes@gmail.com)
