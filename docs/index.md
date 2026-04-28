# Polygon Crusher 9000

![Polygon Crusher 9000](assets/Banner_02.png)

Blender addon for fast, normal-preserving mesh decimation.

[Get Started](getting-started/installation.md) · [Settings Reference](settings.md)

---

## Features

- **Smart Decimation** — high-quality mesh reduction that preserves shape and silhouette at any reduction level.
- **Normal Preservation** — transfers original surface normals onto the result, so smooth shading survives even aggressive crushing.
- **Modifier-Aware** — crushes the *evaluated* mesh; boolean, subdivision, and the rest of the modifier stack are baked in automatically. Originals stay untouched.
- **Tunable Aggressiveness** — dial how hard the simplifier pushes. Lower values keep fine features (eyes, hair, creases); higher values converge faster on lower-detail meshes.
- **Live Progress** — Blender stays responsive during long crushes — heavy work runs on a worker thread. Press **ESC** at any time to abort.
- **Session Stats** — per-object triangle counts and timing displayed after each crush, directly in the panel.

---

## Requirements

| | |
|---|---|
| **Blender** | 4.2 or later (tested on 5.1) |
| **OS** | Windows, macOS, Linux |

---

## Support

Questions or issues? Email [henriquevlopes@gmail.com](mailto:henriquevlopes@gmail.com)
